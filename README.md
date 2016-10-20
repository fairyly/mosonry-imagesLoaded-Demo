# mosonry-imagesLoaded-Demo
预览效果----> [瀑布流简单demo---mosonry+imagesLoaded-Demo](https://fairyly.github.io/mosonry-imagesLoaded-Demo/)



#还有一种是`InfiniteScroll + Masonry + ImagesLoaded`
```javascript
(function() {
	// Main content container
	var $container = $('#content');

	// Masonry + ImagesLoaded
	$container.imagesLoaded(function(){
		$container.masonry({
			// selector for entry content
			itemSelector: '.entry-content',
			columnWidth: 200
		});
	});

	// Infinite Scroll
	$container.infinitescroll({

		// selector for the paged navigation (it will be hidden)
		navSelector  : ".navigation",
		// selector for the NEXT link (to page 2)
		nextSelector : ".nav-previous a",
		// selector for all items you'll retrieve
		itemSelector : ".entry-content",

		// finished message
		loading: {
			finishedMsg: 'No more pages to load.'
			}
		},

		// Trigger Masonry as a callback
		function( newElements ) {
			// hide new items while they are loading
			var $newElems = $( newElements ).css({ opacity: 0 });
			// ensure that images load before adding to masonry layout
			$newElems.imagesLoaded(function(){
				// show elems now they're ready
				$newElems.animate({ opacity: 1 });
				$container.masonry( 'appended', $newElems, true );
			});

	});
	
	/**
	 * OPTIONAL!
	 * Load new pages by clicking a link
	 */

	// Pause Infinite Scroll
	$(window).unbind('.infscr');

	// Resume Infinite Scroll
	$('.nav-previous a').click(function(){
		$container.infinitescroll('retrieve');
		return false;
	});

})();
```
