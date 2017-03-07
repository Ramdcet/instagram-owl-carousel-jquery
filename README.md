<h1>InstaStream</h1>


<h2>Before to use the plugin</h2>
<p>Before to go further be sure you have an Instagram account and an Instagram API Key.</p>
<p>for access token please visit this site <a href="http://instagram.pixelunion.net/"> access token</a> and for   <a href="https://smashballoon.com/instagram-feed/find-instagram-user-id/">  user id </a> </p>




Once you have your Token you can start using this jQuery plugin.
<h2>Installation</h2>
<ol>
	<li>Unzip the folder</li>
	<li>Add the js file to your website </li>
	<li>Add css styles to your website </li>
</ol>

<h2>Configuration</h2>
<p>To call the plugin, you need to write a little javascript/jQuery code</p>

<pre><code>jQuery(document).ready(function ($) {

  $("body").instastream({
		instaToken: 'Your Instagram Token here',
		instaUser: 'Your Instagram User ID here',
		instaResults: 3,
		instaMenu: 'yes'
	});
	
});
</code></pre>


<pre><code>;(function ( $, window, undefined ) {

    // Create the defaults once
    var pluginName = 'instastream',
        document = window.document,
        defaults = {
            instaUser: '4526026830',
            instaResults: 3,
            instaMenu: 'false'
        };
    
    var $nbrResults;
    var $instaUrl;
    var $slideStatus =0;
    // Constructor
    function Plugin( element, options ) {
        this.element = element;
        
        this.options = $.extend( {}, defaults, options) ;      
        this._defaults = defaults;
        this._name = pluginName;        
        this.init();
    }
    
    // Date converter 
		String.prototype.timeconverter=function(){
		    var a = new Date(this*1000);
		    var months = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
		    var year = a.getFullYear();
			  var month = months[a.getMonth()];
			  var date = a.getDate();
			  var time = date+' '+month+' '+year ;
				return time;
		};
		
		// Stream function
		$.fn.createStream = function(slide, target){
		var j = slide;
    	
    	
    	// stream constructor
		  $.ajax({
	    	type: "GET",
			dataType: "jsonp",
			cache: false,
			url: $instaUrl,
			success: function(data) {
				console.log(data);
				
				$(target).append("<div id='sync1' class='owl-carousel owl-theme'></div>");
		    	for (var i = 0; i < $nbrResults; i++) {
				  if (j<20){
					
					if(data.data[j].caption == null){var myCaption = '';} else{var myCaption = data.data[j].caption.text;}
					if (data.data[j].comments.count < 2){var commentLabel = 'commentaire'} else {var commentLabel = 'commentaires'}
					if (data.data[j].likes.count < 2){var likeLabel = 'like'} else {var likeLabel = 'likes'}
				  
					$('.owl-carousel').append("<div class='item'><a target='_blank' href='" + data.data[j].link + "'><img src='" + data.data[j].images.standard_resolution.url + "' alt='" + myCaption + "'></a></div>"); 
					j++;
					$slideStatus = j;
				}
			  };
				var sync1 = $("#sync1");
				var slidesPerPage = 4; //globaly define number of elements per page
				var syncedSecondary = true;
				  sync1.owlCarousel({
					items : 4,
					slideSpeed : 2000,
					nav: true,
					autoplay: true,
					dots: false,
					loop: true,
					responsive: {
								  0: {
									items: 1,
									nav: true
								  },
								  600: {
									items: 3,
									nav: false
								  },
								  1000: {
									items: 5,
									nav: true,
									loop: false,
									margin: 20
								  }
					},
					responsiveRefreshRate : 200,
					navText: ['<svg width="100%" height="100%" viewBox="0 0 11 20"><path style="fill:none;stroke-width: 1px;stroke: #000;" d="M9.554,1.001l-8.607,8.607l8.607,8.606"/></svg>','<svg width="100%" height="100%" viewBox="0 0 11 20" version="1.1"><path style="fill:none;stroke-width: 1px;stroke: #000;" d="M1.054,18.214l8.606,-8.606l-8.606,-8.607"/></svg>'],
				  }).on('changed.owl.carousel', syncPosition);
			}
			});		 
		     
    }
    
    Plugin.prototype.init = function () {
    	// Initial variables
    	$slideStatus =0;
    	$nbrResults =this.options.instaResults;
    	$instaMenu = this.options.instaMenu;
    	$instaUrl = 'https://api.instagram.com/v1/users/' + this.options.instaUser + '/media/recent/?access_token=' + this.options.instaToken;
	    var $myContainer = this.element;
		$().createStream($slideStatus,$myContainer);
    };

    $.fn[pluginName] = function ( options ) {
        return this.each(function () {
            if (!$.data(this, 'plugin_' + pluginName)) {
                $.data(this, 'plugin_' + pluginName, new Plugin( this, options ));
            }
        });
    };
}(jQuery, window));
 
 function syncPosition(el) {
    var count = el.item.count-1;
    var current = Math.round(el.item.index - (el.item.count/2) - .5);
    
    if(current < 0) {
      current = count;
    }
    if(current > count) {
      current = 0;
    }
}

</code></pre>

<p>You can choose how many pictures you want by slide. CSS cover 1,2,3 and 4 results, feel free to add new styles if you want more results by slide.</p>

<p>Be aware that the JSON file that the plugin read from Instagram only shows 20 pictures.</p>

<p>That's it, have fun !</p>

