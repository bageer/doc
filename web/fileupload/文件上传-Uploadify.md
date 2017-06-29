# 文件上传 #

目前我们公司通用的文件上传插件，是通过异步的方式上传文件的，上传的文件有可能存放在应用中、Fastdfs文件服务器或者阿里云服务器(视具体的应用定义没有特殊说明上传文件均存放在fastdfs文件服务器)，具体插件展现形式如下图所示：

![](http://i.imgur.com/rYGfgYl.png)

使用该插件按照如下几个步骤操作：

1、引入文件上传依赖的插件：

	<script type="text/javascript">
	    var jsLibArr = ["fxUploadify"];
	</script>

	<%@include file="/WEB-INF/jsp/common/include.jsp" %>

说明：前三行是定义需要引入文件上传依赖的插件，第四行一般是定义所有页面公共引入的资源，是所有页面都需要引入的。

2、在form表单中定义文件上传标签，代码如下图所示：

	<div class="form-group">
	     <label class="col-sm-2 control-label">LOGO</label>
	     <div class="col-sm-10">
	         <div class="btn btn-primary ybtn-primar upload_panel cus_upload_btn" resType="0" style="height:36px;"
	              uploadifyUrl="${ctx}/manage/upload/pic.json" <!--上传文件地址-->
	              progressPanel="#progressPanel" <!--上传进度条0-->
	              name="file" <!--上传表单名称-->
	              initLoader="true" <!--页面初始化完毕是否初始化上传组件：true是，false否-->
	              fileCount="1" <!--最多选择文件个数-->
	              fileSize="5MB" <!--文件大小限制5MB，800KB，1GB等-->
	              fileType=".jpg,.jpeg,.gif,.png" <!--上传文件类型限制-->
	              selectFileOver="selectFileOver" <!--选择完毕文件回调函数-->
	              oneCallback="uploadOverOne" <!--某一个文件上传完毕回调函数-->
	              callback="uploadOverCallback" <!--所有文件上传完毕以后回调函数-->
	             >上传LOGO
	         </div>
			 <!--以下三项一般放在这里就可以了，如果需要在其他特殊位置展示讲代码移动到相应的位置即可-->
	         <div id="progressPanel"></div> <!--上传的进度条ID要和进度条参数中定义的ID保持一致-->
	         <input type="hidden" name="picId" value=""/> <!--文件上传完毕以后回填的值-->
	         <div class="uploadResult" style="display:none;margin-top:10px;"></div> <!--文件上传成功以后展示的上传结果-->
	     </div>
	 </div>

3、后台添加处理文件上传的业务类，其中文件上传以上传到Fastdfs为例，其他情况只需修改FileUploadUtils.uploadToFastdfs这个方法即可，FileUploadUtils工具类中提供了上传到应用，服务器自定义文件夹，Fastdfs中的相关方法。

	//后台处理文件上传的Controller

	@Controller
	@RequestMapping("/manage/upload")
	public class FileUploadController {
	
	    @Value(value = "#{config['upload.pic.allowed.extension']}")
	    private String picAllowExtension;
	    @Value(value = "#{config['upload.pic.allowed.maxSize']}")
	    private long picAllowMaxSize;
	
	    @Autowired
	    private IFxtxFileService fxtxFileService;
	
	
	    @Value(value = "#{config['img.upload.ctx']}")
	    private String imgCtx;
	    private String[] showFileProperties = new String[]{"fileName","fileUrl","suffix","size","id"};
	
	
	    /**
	     * 上传处理方法
	     */
	    @RequestMapping(value = "/pic", method = RequestMethod.POST)
	    public Map uploadPic(MultipartFile file, HttpServletResponse response,Model model) {
	        //上传文件
	        UploadResult uploadResult = FileUploadUtils.uploadToFastdfs(file, picAllowExtension, picAllowMaxSize);
	        return getResult(uploadResult);
	    }
	    
	    /**
	     * 返回文件结果
	     *
	     * @param uploadResult
	     * @return
	     */
	    public Map<String,Object> getResult(UploadResult uploadResult){
	        AdminUser adminUser = AdminUserUtils.getUser();
	        Map<String, Object> map = Maps.newHashMap();
	        FxtxFile fxFile = fxtxFileService.createByUploadResult(uploadResult,adminUser);
	        if(fxFile!=null){
	            map.put("flag",1);
	            map.put("info",fxtxFileService.convertToMap(fxFile,showFileProperties));
	        }else{
	            map.put("flag",0);
	            map.put("msg",uploadResult.getMessage());
	        }
	        return map;
	    }
	}

	//后台接受文件上传的逻辑层
	@Service
	@Transactional
	public class FxtxFileServiceImpl extends BaseFxtxFileServiceImpl implements IFxtxFileService {
	
	    @Value(value = "#{config['img.upload.ctx']}")
	    private String imgCtx;
	
	    /**
	     * 根据上传结果添加文件记录
	     *
	     * @param result
	     */
	    @Override
	    public FxtxFile createByUploadResult(UploadResult result, AdminUser adminUser){
	        FxtxFile file = new FxtxFile();
	        FileMeta fileMata = result.getMata();
	        if(fileMata!=null && result.getFlag()){
	            file.setFileName(fileMata.getFileName());
	            file.setSuffix(fileMata.getExtension());
	            file.setSize(fileMata.getSize());
	            file.setAddTime(new Date());
	            file.setAddUserId(adminUser == null ? 0L:adminUser.getId());
	            file.setFilePath(result.getPath());
	            if(StringUtils.isNotEmpty(result.getPath())){
	                file.setFileUrl(WebPathUtils.getFullUrl(imgCtx,result.getPath()));
	            }
	            file.setMimeType(fileMata.getMineType());
	            file.setStorageType(result.getStorageType());
	            create(file);
	            return file;
	        }
	        return null;
	    }
	}

	
