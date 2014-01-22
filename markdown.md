###Image Carousel
The following code uses the plugin *flexslider.js* to initiate the slideshow. The parameters can be modified as needed. For a full list of available options, visit <http://www.woothemes.com/flexslider>.

    $('.flexslider').flexslider({
        animation: "slide",
        controlNav: false,
        startAt: 0,
        mousewheel: true,
        animationSpeed: 300,
        initDelay: 5000,
		slideshow: true,
        slideshowSpeed: 5000
    });
    
For the markup, start with a single containing div with a class of *flexslider*. Then create an unordered list with a class of *slides*. 

    <div class="flexslider">
      <ul class="slides">
        <li><img src="slide1.jpg"/></li>
        <li><img src="slide2.jpg"/></li> 
      </ul>
    </div>

###Click to play video
Code snippet for click to play upon expand (where the click event causes the banner to expand). For click to play, we can use the html5 video tag together with javascript. Example from a recent campaign: <http://phad.phluant.net/index/demo/id/fd7a8412a41de25682c68bae9d8eff21edfb3271>

The Javascript: Expand banner in response to the click event

    $("#mini-bg").click(function() {
	    $("#ph_conBanner").css("display", "none");
	    $("#expand").css("opacity", 1);
	    var video = document.getElementById('video');
	    video.play();
    });

    $("#close").click(function() {
	    $("#ph_conBanner").css("display", "block");
	    //show the original banner image when expanded banner is closed
	    $("#expand").css("opacity", 0);
	    //hide the div with the id of expand
	    var video = document.getElementById('video');
	    video.pause();
    });


The HTML		
			
    <!--320x50 banner image-->
    <div id="ph_conBanner"><img src="img/320x50.png" id="mini-bg" class="absolute"></div> 
    <!--expanded banner-->
    <div id="expand">					
	<div><img src="img/x.png" id="close"></div>
		
	<div> <!--video container-->
		<video class="absolute" id="video" width="320" height="179" controls>  
		  	<source src="video.webm" type='video/webm'>
			<source src="video.mp4" type='video/mp4'>
		</video>		
    </div>	
    
    
###Autoplay inline video
This feature is demonstrated in a recent proof of concept: <http://phad.phluant.net/index/demo/id/f7d55e202e6b5f9e88fad70f870ded6597ed38a4> To enable autoplay on IOS, it is necessary to render the video as a series of discrete images, then show each frame in succession. 

    //place this loop in the init function
    for(var i=1;i<=500;i++){
	    (function(i){
				var img = new Image();
				img.onload = function(){
					loadedNumber++;
					if(loadedNumber == 60){
						isLoaded=true;
						play();
					}
				}
				img.src = 'http://mdn2.phluantmobile.net/../video/video'+i+'.jpg';
				arrImg.push(img);
			}
		)(i);
	  }
    }	
    
    function play(){
        if(!isLoaded){ setTimeout(play,200); return; };
        if(ph.u.isIOSDevice()||ph.u.isAndroid()){if(!isMuted) phAudio.play(); } 
        else phAudio.play();
        try{ if(audio.buffered.end(0)<12) framereq=75; } catch(e){};
        playVideo();
    }

    var index=0;
    var canvas  = document.getElementById('video');
    var context = document.getElementById('video').getContext('2d');
    
    function playVideo(){
	if(isPlaying) return; isPlaying = true;
	index = 0;
	(function drawVideo(){
		if(!isPlaying) return;
		context.clearRect(0, 0, canvas.width, canvas.height);
        if(index<=498) index+=2;
		try{  context.drawImage(arrImg[index],0,0,arrImg[index].width,arrImg[index].height,0,0,320,240); } catch(e){};
		if(index == 500){
			context.drawImage(arrImg[30],0,0,arrImg[30].width,arrImg[30].height,0,0,320,240);
			isEnd = true;
			fadeInImg('replay',0.5);
		}
		else
			window.requestAnimationFrame(drawVideo);
	}());
    } 


###Autoplay 3sec inline video in banner
This feature can be seen in a recent campaign: <http://test1.phluant.com/Mercedes_new/index.html>


    function drawVideoBanner(){
	fadeOutImg('banner_text',0.5);
	fadeInImg('video_banner',0.5);
	setTimeout(function(){
		var canvas  = document.getElementById('video_banner');
		var context = document.getElementById('video_banner').getContext('2d');
		var index = 0;
		_drawVideoBanner(canvas,context,index);
	},500);
    }

    function drawVideoBannerIE(){
	fadeOutImgIE('banner_text',0.5);
	fadeInImgIE('video_banner',0.5);
	setTimeout(function(){
		var canvas  = document.getElementById('video_banner');
		var context = document.getElementById('video_banner').getContext('2d');
		var index = 0;
		_drawVideoBannerIE(canvas,context,index);
	},500);
    }

    function hideVideoBanner(){
	fadeOutImg('video_banner',0.5);
	setTimeout(function(){
		fadeInImg('banner_text',0.5);
	});
    }

    function hideVideoBannerIE(){
	fadeOutImgIE('video_banner',0.5);
	setTimeout(function(){
		fadeInImgIE('banner_text',0.5);
	});
    }

    //Place this snippet in the document.ready function:
    for(var i=188;i<=221;i++){
		(
			function(i){
				var img = new Image();
				img.onload = function(){
					loadedNumber++;
					if(loadedNumber == numberImg){
						isLoaded = true;
						if(canPlayVideoBanner)	{
							if($.browser.msie){
								drawVideoBannerIE();
							}
							else{
								drawVideoBanner();
							}
						}
					}
				}
				img.src = 'video/video'+i+'.jpg';
				arrImg.push(img);
			}
		)(i);
	}
	