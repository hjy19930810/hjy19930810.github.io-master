## 滚动条回顶
方法1：jquery
```html
<div class="scroll" id="scroll" style="display: none;">回到顶部</div>
<!--这句话生成一个回顶的点击区，并且让他隐藏-->
```
```css
 .scroll{
    width: 70px;
    height: 70px;/*点击区大小*/
    background: #64bfae;
    color: #fff;
    line-height: 70px;
    text-align: center;
    position: fixed;
    right:30px;
 	bottom:50px;
 	/*设置position: fixed;然后设置right:30px;bottom:50px;使点击区置于页面右下角*/
 	cursor: pointer;
 	font-size:14px;
	 }
```
```javascript
$(function(){
	$(window).scroll(function(){
		var scrollValue=$(window).scrollTop();//使用scrollTop获取滚动条到页面顶的距离
		scrollValue>100?$('div.scroll').fadeIn():$('div.scroll').fadeOut();
	})//如果距离大于100，则使点击区渐渐显示，如果小于100，则渐渐消失
	$('#scroll').click(function(){
		$('html,body').animate({scrollTop:0},200)
	})//经历200ms，到达顶端
})
```
#### 注意：jquery中fadeIn()和fadeOut()函数只适用于display:none 的元素，与js不一样，js中想要通过transition来慢慢显示或慢慢隐藏元素，元素只能是 visibility:hidden。这与display破坏transition，因为display会导致reflow。

方法2：js原生
```html
<div class="scroll" id="scroll" >回到顶部</div>
```
```css
.scroll{
	visibility:hidden;
	/*不要使用display：none*/
    width: 70px;
    height: 70px;
    background: #64bfae;
    color: #fff;
    line-height: 70px;
    text-align: center;
    position: fixed;
    right:30px;
 	bottom:50px;
 	cursor: pointer;
 	font-size:14px;
	transition: all 300ms; 
	-moz-transition:all 300ms; /* Firefox 4 */
	-webkit-transition: all 300ms; /* Safari 和 Chrome */
	-o-transition:all 300ms; /* Opera */
	/*这里使用transition，就是我了过渡*/
	 }
```
```javascript
function fadeIn(){
		scroll.style.visibility='visible';
		scroll.style.opacity=1;
	}
	function fadeOut(){
		scroll.style.visibility='hidden';
		scroll.style.opacity=0;
	}
		window.onload=function(){
			scroll=document.getElementById('scroll');//scroll设置成全局变量，才能调用fadeOut()和fadeIn()
			var timer=null;
			var isTop=true;	
		window.onscroll=function(){
			var scrollTop=document.documentElement.scrollTop || document.body.scrollTop;			
//scrollTop>100?scroll.style.visibility='visible':scroll.style.visibility='hidden';
//可以使用上面这句，它与下面的if  else  等
//可以使用visibility,它是repaint;
//scrollTop>100?scroll.style.display='block':scroll.style.display='none';
//不能用display,因为display破坏transition，它是reflow;如果想用，可以看这个解决方法：http://www.cnblogs.com/ihardcoder/p/3859026.html
			if(scrollTop>100)
			{
				fadeIn();
			}
			else
			{
				fadeOut();
			}
		}
		scroll.onclick=function(){
			timer=setInterval(function(){
				var osTop = document.documentElement.scrollTop || document.body.scrollTop;    
 				var ispeed = Math.floor(-osTop / 7);     
 				document.documentElement.scrollTop = document.body.scrollTop = osTop+ispeed;   
 				//以上是为了兼容性，使用了document.documentElement.scrollTop || document.body.scrollTop;
				//到达顶部，清除定时器    
 				if (osTop == 0) {   
				 clearInterval(timer);      
 				};
 				isTop=true;
			},30);
		};
		};
```
