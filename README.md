# kissy
一个下午回顾下kissy框架，实现各小功能,改进中，欢迎吐槽————hellowrold3q3q@gmail.com

用kissy写的一个h5页面滑动

图片文件地址：
http://pan.baidu.com/s/1jGL73PO
密码：51y7

kissy api: http://docs.kissyui.com/

kissy 代码

/*×配置start**/
var sldPara={
	distanceP:40, //滑动距离
	cycle:true, //是否回到第一页
}
/*×endstart**/


KISSY.add('sliderDiv',function(){
	KISSY.use("node,dom,event,anim",function(S,Node,DOM,Event,Anim){
		var tfm_cple=0,
			$ = Node.all,
			$page = $('.page'),
			$slider = $('#slider'),
			page_leg = $page.length,
			windowHight = parseInt($(window).height()),
			slide_dtce = sldPara.distanceP;

		Event.on('#slider',"swipe",function(e){
			S.log(e.type + ' : fired');
            S.log(e.direction);
            S.log(e.distance);
            if(e.direction=="down" && e.distance >slide_dtce){
            	runAnim("down");
            }else if(e.direction=="up" && e.distance >slide_dtce){
            	 runAnim("up");
            }
		});

		function tfmId(){
			var now_tfm = tfm_cple/windowHight+1;
			return now_tfm;
		}

		function runAnim(tfmDtc){
			var now_tfm = tfmId();
			if(tfmDtc=="down" ){
   				var tfmUp = tfm_cple-windowHight;
       			tfmAnim(now_tfm-1,tfmUp,"down");
       		}else if(tfmDtc=="up"){
       		    var tfmDown = tfm_cple+windowHight;
				if(now_tfm==page_leg && sldPara.cycle==true){
					 tfmAnim(now_tfm,0,"up");	
				}else if(now_tfm!=page_leg){
					 tfmAnim(now_tfm,tfmDown,"up");
				}	
       		}
		}

		function tfmAnim(addTarget,tfmHeight,tfmDtc){
			$slider.animate(
				{
					transform:"translateY(-"+tfmHeight+"px)"				
				},
				{
					complete:function(){
						tfm_cple =  tfmUtil();
					}
				}
			);
			addActive(addTarget,"slider-active",tfmDtc);
		}

		function tfmUtil(){
			var page_tfm = DOM.css("#slider","transform");
     		var reg = /[1-9][0-9]*/g;  
            var num = parseInt(page_tfm.match(reg)[2]); 
            if(isNaN(num)){
            	return 0;
            }else{
            	return num;
            }
			
		}

		function addActive(addTarget,activeClass,tfmDtc){
			var prvTgt = addTarget-1;
			var nextTgt = addTarget+1;
			if(tfmDtc == "up"){
				if(addTarget==page_leg){
					$page.item(0).addClass(activeClass);
				}else{
					$page.item(addTarget).addClass(activeClass);
				}
				$page.item(prvTgt).removeClass(activeClass);
				if(nextTgt<page_leg){
					$page.item(nextTgt).removeClass(activeClass);
				}
			}else if(tfmDtc=="down"){
				$page.item(prvTgt).addClass(activeClass);
				$page.item(addTarget).removeClass(activeClass);
			}
		}

	});
});


KISSY.ready(function(S){
	KISSY.use("node,dom",function(S,Node,DOM){
		var $ = Node.all;
		var windowHight = $(window).height();
		DOM.css('.page , #container',{height:windowHight+'px'});

	});
	KISSY.use('sliderDiv',function(){});

});
