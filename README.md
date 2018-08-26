# uniapp-picker
uniapp选择器,包含一级,二级级联,三级级联
![一级选择](https://xywqqcom.oss-cn-shenzhen.aliyuncs.com/153530726873479.jpg)
![2级选择](https://xywqqcom.oss-cn-shenzhen.aliyuncs.com/153530727498451.jpg)
![3级选择](https://xywqqcom.oss-cn-shenzhen.aliyuncs.com/153530727967879.jpg)

**github:[github](https://github.com/lygustas/uniapp-picker)**

已知问题:
1. 不能与页面下拉一起使用
2. 滑动选择后,scrollview滚动会有500ms左右的延迟

参数说明:
show(类型:Boolean,默认 false):控制组件显示隐藏

list(类型:Array):选择器绑定的数据

type(类型:Number,取值1,2,3):选择器类型 1为一级选择 2为二级级联选择 3为三级级联选择

name(类型:String,默认:'name'):选择器显示的文本信息,为list中的某个key

children(类型:String,默认:'children'):级联选择器中下级选择器从上级选择器获取数据时所使用的key,为list中的某个key

callBackFun:回调方法,回调参数类型:Array,一级选择器确定后的回调值为Array[0],Array[0]为所选项的Object
                   二级选择器确定后的回调值为Array[0,1],Array[0]为所选第一项的Object,Array[1]为所选第二项的Object
                   三级选择器确定后的回调值为Array[0,1,2],Array[0]为所选第一项的Object,Array[1]为所选第二项的Object,Array[2]为所选第二项的
                   Object
详细使用方法见demo
