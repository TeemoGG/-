# -

<!DOCTYPE html> 
<html> 
<head> 
<meta charset="UTF-8"> 
<title>jquery轮播效果图 </title> 
<script type="text/javascript" src="js/jquery-1.8.3.min.js"></script> 
<style type="text/css"> 
 * { 
 padding: 0px; 
 margin: 0px; 
 } 
 a { 
 text-decoration: none; 
 } 
 ul { 
 list-style: outside none none; 
 } 
 .slider, .slider-panel img, .slider-extra { 
 width: 650px; 
 height: 413px; 
 } 
 .slider { 
 text-align: center; 
 margin: 30px auto; 
 position: relative; 
 } 
 .slider-panel, .slider-nav, .slider-pre, .slider-next { 
 position: absolute; 
 z-index: 8; 
 } 
 .slider-panel { 
 position: absolute; 
 } 
 .slider-panel img { 
 border: none; 
 } 
 .slider-extra { 
 position: relative; 
 } 
 .slider-nav { 
 margin-left: -51px; 
 position: absolute; 
 left: 50%; 
 bottom: 4px; 
 } 
 .slider-nav li { 
 background: #ccc; 
 border-radius: 50%; 
 color: black; 
 cursor: pointer; 
 margin: 0 2px; 
 overflow: hidden; 
 text-align: center; 
 display: inline-block; 
 height: 20px; 
 line-height: 20px; 
 width: 20px; 
 } 
 .slider-nav .slider-item-selected { 
 background: #eee; 
 } 
 .slider-page a{ 
 background: rgba(0, 0, 0, 0.2); 
 color: #fff; 
 text-align: center; 
 display: block; 
 font-size: 22px; 
 width: 28px; 
 height: 62px; 
 line-height: 62px; 
 margin-top: -31px; 
 position: absolute; 
 top: 50%; 
 } 
 .slider-page a:HOVER { 
 background: rgba(0, 0, 0, 0.4); 
 } 
 .slider-next { 
 left: 100%; 
 margin-left: -28px; 
 } 
</style> 

</head> 
<body> 
 <div class="slider"> 
   <ul class="slider-main"> 
      <li class="slider-panel"> 
      <a href="http://www.jb51.net" target="_blank"><img alt="关注脚本之家" title="关注脚本之家" src="img/2.jpg"></a> 
      </li> 
      <li class="slider-panel"> 
      <a href="http://www.jb51.net" target="_blank"><img alt="关注脚本之家" title="关注脚本之家" src="img/3.jpg"></a> 
      </li> 
      <li class="slider-panel"> 
      <a href="http://www.jb51.net" target="_blank"><img alt="关注脚本之家" title="关注脚本之家" src="img/10.jpg"></a> 
      </li> 
      <li class="slider-panel"> 
      <a href="http://www.jb51.net" target="_blank"><img alt="关注脚本之家" title="关注脚本之家" src="img/12.jpg"></a> 
      </li> 
   </ul> 
   <div class="slider-extra"> 
      <ul class="slider-nav"> 
        <li class="slider-item">1</li> 
        <li class="slider-item">2</li> 
        <li class="slider-item">3</li> 
        <li class="slider-item">4</li> 
      </ul> 
      <div class="slider-page"> 
        <a class="slider-pre" href="javascript:;;"><</a> 
        <a class="slider-next" href="javascript:;;">></a> 
      </div> 
   </div> 
 </div> 
</body> 
<script type="text/javascript"> 
 $(document).ready(function() { 
 var length, 
  currentIndex = 0, //下标
  interval, 
  hasStarted = false, //是否已经开始轮播 
  t = 3000; //轮播时间间隔 
 length = $('.slider-panel').length; 
 //将除了第一张图片隐藏 
 $('.slider-panel:not(:first)').hide(); 
 //将第一个slider-item设为激活状态 
 $('.slider-item:first').addClass('slider-item-selected'); 
 //隐藏向前、向后翻按钮 
 $('.slider-page').hide(); 
 //鼠标上悬时显示向前、向后翻按钮,停止滑动，鼠标离开时隐藏向前、向后翻按钮，开始滑动 
 $('.slider-panel, .slider-pre, .slider-next').hover(
  function() { 
      stop(); 
      $('.slider-page').show(); 
  }, 
    function() { 
      $('.slider-page').hide(); 
      start(); 
  }); 

//这个是图片下的小圈圈
 $('.slider-item').hover(
  function() { 
      stop(); 
      //当前下标数.filter()遍历slider-item，.index()获取当前鼠标在slider-item的下标
      var preIndex = $(".slider-item").filter(".slider-item-selected").index(); 
      //currentIndex图片的下标
      currentIndex = $(this).index(); 
      play(preIndex, currentIndex); 
  }, 
    function() { 
      start(); 
 }); 

 $('.slider-pre').unbind('click'); 
 $('.slider-pre').bind('click', function() { 
  pre(); 
 }); 

 $('.slider-next').unbind('click'); 
 $('.slider-next').bind('click', function() { 
  next(); 
 }); 
 /** 
  * 向前翻页 
  */
 function pre() { 
  //当前的图片和小圈的下标
  var preIndex = currentIndex; 
  //下个翻页的图片和小圈的下标
  currentIndex = (--currentIndex + length) % length; 
  alert("当前的图片和小圈的下标:"+preIndex+'---'+'下个翻页的图片和小圈的下标:'+currentIndex)
  play(preIndex, currentIndex); 
 } 
 /** 
  * 向后翻页 
  */
 function next() { 
  var preIndex = currentIndex; 
  currentIndex = ++currentIndex % length; 
  play(preIndex, currentIndex); 
 } 
 /** 
  * 从preIndex页翻到currentIndex页 
  * preIndex 整数，翻页的起始页 
  * currentIndex 整数，翻到的那页 
  */
 function play(preIndex, currentIndex) { 
  $('.slider-panel').eq(preIndex).fadeOut(500) 
  .parent().children().eq(currentIndex).fadeIn(1000); 
  $('.slider-item').removeClass('slider-item-selected'); 
  $('.slider-item').eq(currentIndex).addClass('slider-item-selected'); 
 } 
 /** 
  * 开始轮播 
  */
 function start() { 
  if(!hasStarted) { 
  hasStarted = true; 
  interval = setInterval(next, t); 
  } 
 } 
 /** 
  * 停止轮播 
  */
 function stop() { 
  clearInterval(interval); 
  hasStarted = false; 
 } 
 //开始轮播 
 start(); 
 }); 
</script> 
</html>
