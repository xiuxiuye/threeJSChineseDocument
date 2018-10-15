
function str2asc(strstr) {
    return ("0" + strstr.charCodeAt(0).toString(16)).slice(-2);
}

function asc2str(ascasc) {
    return String.fromCharCode(ascasc);
}

function urlencode(str) {
    var ret = "";
    var strSpecial = "!\"#$%&'()*+,/:;<=>?[]^`{|}~%";
    var tt = "";

    for (var i = 0; i < str.length; i++) {
        var chr = str.charAt(i);
        var c = str2asc(chr);
        tt += chr + ":" + c + "n";
        if (parseInt("0x" + c) > 0x7f) {
            ret += "%" + c.slice(0, 2) + "%" + c.slice(-2);
        } else {
            if (chr == " ")
                ret += "+";
            else if (strSpecial.indexOf(chr) != -1)
                ret += "%" + c.toString(16);
            else
                ret += chr;
        }
    }
    return ret;
}

function urldecode(str) {
    var ret = "";
    for (var i = 0; i < str.length; i++) {
        var chr = str.charAt(i);
        if (chr == "+") {
            ret += " ";
        } else if (chr == "%") {
            var asc = str.substring(i + 1, i + 3);
            if (parseInt("0x" + asc) > 0x7f) {
                ret += asc2str(parseInt("0x" + asc + str.substring(i + 4, i + 6)));
                i += 5;
            } else {
                ret += asc2str(parseInt("0x" + asc));
                i += 2;
            }
        } else {
            ret += chr;
        }
    }
    return ret;
}

function validateEmail(email){
	var emailReg = /^([\w-\.]+@([\w-]+\.)+[\w-]{2,4})?$/;
	if(!emailReg.test(email))
		return false;
	else
		return true;
}

function trim(str){
    return str.replace(/(^\s*)|(\s*$)/g, '');
}
function empty(str) {
    return (!str || 0 === str.length);
}
function isURL(url) {
    var strRegex = "^((https|http|ftp|rtsp|mms)?://)"
        + "?(([0-9a-z_!~*'().&=+$%-]+: )?[0-9a-z_!~*'().&=+$%-]+@)?" //ftp user@
        + "(([0-9]{1,3}\.){3}[0-9]{1,3}" // URL in IP format- 199.194.52.184
        + "|" // allowed ip and domain name
        + "([0-9a-z_!~*'()-]+\.)*" // domain name- www.
        + "([0-9a-z][0-9a-z-]{0,61})?[0-9a-z]\." // second level domain name
        + "[a-z]{2,6})" // first level domain- .com or .museum
        + "(:[0-9]{1,4})?" // port- :80
        + "((/?)|" // a slash isn't required if there is no file name
        + "(/[0-9a-z_!~*'().;?:@&=+$,%#-]+)+/?)$";
     var re=new RegExp(strRegex);
     return re.test(url);
}
function isJsCss(url) {
    var strRegex = "(js|css)$";
    var re=new RegExp(strRegex);
    return re.test(url);
}
function rand(){ 
    var Num=""; 
    for(var i=0;i<6;i++) 
    { 
        Num += Math.floor(Math.random()*10); 
    }
    return Num;
}
function serialize(array){
	var str="";
	for(var t in array){
		str+=t+"="+array[t]+","
	}
	return str.substring(0,str.length-1);
}
function unserizalize(str){
	var array={};
	var tem=str.split(",");
	for(var i=0;i<tem.length;i++){
		var obj=tem[i].split("=");
		var val=obj[1];
		if($.isNumeric(val)){
			val=parseFloat(val);
		}
		array[obj[0]]=val;
	}
	return array;
}
$(function(){
	  SandBox=(function(){
		function SandBox(el,options){
			this.options=options;
			this.$wrap=$(options.wrapSelector);
			this.$drag=$(el);
			this.$prev=$(el).prev();
			this.$next=$(el).next();
			this.prevWidthRate = this.options.prevWidthRate;
			this.prevHeightRate =  this.options.prevHeightRate;

			var options=this.options;
			var cursor="";
			if( options.hDrag ){
				cursor="e-resize";
			}
			else if( options.vDrag ){
				cursor="n-resize";
			}
			this.$drag.css({"cursor":cursor});
			if(!$(".floor")[0]){
				$("<div class='floor'></div>").css({'width':'100%','height':'100%','position':'absolute','z-index':9999,'opacity':0,'left':0,'top':0,'background-color':'red'}).hide().appendTo("body");
			}
			this.initWrap();
			this.init();
			this.initEvents();
			this.addEvents();
		}
		SandBox.prototype.initWrap=function(){
			this.windowWidth = window.innerWidth?window.innerWidth:$(window).width();
			this.windowHeight = window.innerHeight?window.innerHeight:$(window).height();
			this.$wrap.height( this.windowHeight - $(".navbar-inverse").height());
		};
		SandBox.prototype.init=function(){
			this.mouseX=0;
			this.mouseY=0;
			
			if( this.options.hDrag ){
				var w=this.$drag.parent().width() - this.$drag.width();
				this.prevWidth=w * this.prevWidthRate ;
				this.nextWidth=w - this.prevWidth;
//preserve 1 px for safe
				this.$prev.width( this.prevWidth - 1 );
				this.$next.width( this.nextWidth - 1 );
			}else if( this.options.vDrag ){
				var h=this.$drag.parent().height() - this.$drag.height();
				this.prevHeight=h * this.prevHeightRate;
				this.nextHeight=h - this.prevHeight;

				this.$prev.height( this.prevHeight );
				this.$next.height( this.nextHeight );
			}			
		};
		SandBox.prototype.initEvents=function(){
			var options=this.options;
			this.events = {
				down: (function (_this) {
					return function (e) {
						_this.mouseX=e.pageX;
						_this.mouseY=e.pageY;

						$(".floor").show().bind("mousemove", _this.events["drag"]).bind("mouseup", _this.events["up"]);
						return false;
					};
				})(this),
				drag: (function (_this) {
					return function (e) {
						var $prev=_this.$prev;
						var $next=_this.$next;
						
						if( _this.options.hDrag){
							var mouseX=e.clientX;
							var moveX = mouseX-_this.mouseX;

							var prevWidth = _this.prevWidth += moveX;
							var nextWidth = _this.nextWidth -= moveX;

							if( prevWidth<options.minWidth || nextWidth<options.minWidth ){
								return false;
							}

							$prev.width( prevWidth );
							$next.width( nextWidth );

							_this.mouseX=mouseX;
						}
						if( _this.options.vDrag){
							var mouseY=e.clientY;
							var moveY = mouseY-_this.mouseY;

							var prevHeight = _this.prevHeight += moveY;
							var nextHeight = _this.nextHeight -= moveY;

							if( prevHeight<options.minHeight || nextHeight<options.minHeight){
								return false;
							}

							$prev.height( prevHeight );
							$next.height( nextHeight );

							_this.mouseY=mouseY;
						}
						
						return false;
					};
				})(this),
				up: (function (_this) {
					return function (e) {
						_this.currentDrag=null;
						$(".floor").hide().unbind("mousemove", _this.events["drag"]).unbind("mouseup", _this.events["up"]);
						return false;
					};
				})(this)
			};
		};
		SandBox.prototype.addEvents=function(){
			var events;
			this.removeEvents();
			events = this.events;
			this.$drag.bind("mousedown", events["down"]);
		};
		SandBox.prototype.removeEvents=function(){
			var events;
			events = this.events;
			this.$drag.unbind();
		};
		SandBox.prototype.reset=function(){
			this.updateRate();
			this.initWrap();
			this.init();
		};	
		SandBox.prototype.updateRate=function(){
			if( this.options.hDrag ){
				var prevWidthRate = this.$prev.width()/(this.$prev.width()+this.$next.width());
				this.prevWidthRate = prevWidthRate;
			}
			if( this.options.vDrag ){
				var prevHeightRate =  this.$prev.height()/(this.$prev.height()+this.$next.height());
				this.prevHeightRate = prevHeightRate;
			}
		};
		SandBox.prototype.saveCookie=function(){
			this.updateRate();
			var str=serialize($.extend(this.options,{
				prevWidthRate:this.prevWidthRate,
				prevHeightRate:this.prevHeightRate
			}));
			$.cookie(this.$drag.attr("id"), str ,{expires:7});	
		};
		return SandBox;
	  })();
	  $.fn.sandbox = function (settings) {
		return this.each(function() {
			var sandbox,options;
			if (!(sandbox=this.m_sandbox)) {
				options = $.extend({}, $.fn.sandbox.defaults, settings);
				return this.m_sandbox = sandbox = new SandBox(this,options);
			}
			if (settings && typeof settings === "object") {
                $.extend(sandbox.options, settings);
				sandbox.reset();
			}else if( settings === "reset"){
				sandbox.reset();
			}else if( settings === "saveCookie"){
				return sandbox.saveCookie();
			}
		});
	  };
	  //horizontal,vertical
	  $.fn.sandbox.defaults={
		minWidth:0,
		minHeight:0,
		hDrag:0,
		prevWidthRate:0.5,
		vDrag:0,
		prevHeightRate:0.3,
		wrapSelector:".wrap"
	  };
});