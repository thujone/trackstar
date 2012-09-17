trackstar
=========
This is a very simple, lightweight plugin that lets you show an overlay that follows your mouse movement when hovering over an element on a page. Mousing out makes the overlay disappear. I wrote this because I didn't see anything out there that did just this. The Cluetip plugin, though wonderful in other ways, is much too heavy for simple overlay with mouse tracking.

HTML:

<div id="show-more">hover over me!</div>
<div id="overlay">
    overlay content, yay!
    (this can contain anything...
    images, text, other containers, etc.)
</div>
JavaScript:

$('#show-more').trackStar( {displayID: 'overlay'} );
CSS:

#overlay {
    display: none;
}
When the user hovers over an element, TrackStar clones the overlay content, copies that into a container div, and absolutely positions that div near the mouse position. The position can be tweaked with two optional plugin parameters.

$('#show-more').trackStar( {
    displayID: 'overlay',
    xOffset: 20,  // display 20 pixels right of the cursor
    yOffset: 10   // display 10 pixels above the cursor
} );
That's it! You can check out a working example here:

http://wgt.com/proshop/products.aspx?b=e0948fff-d346-4d54-9035-3fe5ac6286ed

Email me (rgoldman -*at*- worldgolftour -*dotcom*-) if you have any questions or comments.

Here's the code. Save as 'jquery.trackstar.js'.

/*
* TrackStar
* A simple tooltip with reliable mouse tracking
*
*/
(function($) {
    $.fn.trackStar = function(options) {
        debug(this);
        var opts = $.extend({}, $.fn.trackStar.defaults, options);
        return this.each(function() {
            $this = $(this);
            var o = $.meta ? $.extend({}, opts, $this.data()) : opts;
            $this.hover(function(event) {
                $('body').append('<div id="preview" style="position: absolute; background: #fff"></div>');
            $('#' + opts.displayID).clone().css('id', '').appendTo($('#preview')).show();
            $('#preview')
                    .css('top', (event.pageY - opts.xOffset) + 'px')
                    .css('left', (event.pageX + opts.yOffset) + 'px')
                    .fadeIn('fast');
        }, function(event) {
            $('#preview').remove();
        });
        $this.mousemove(function(event) {
            $('#preview')
                    .css('top', (event.pageY - opts.xOffset) + 'px')
                    .css('left', (event.pageX + opts.yOffset) + 'px');
        });
        });
    };
    function debug($obj) {
        if (window.console && window.console.log) {
             ;//console.log();
        }
    };
    // plug-in defaults
    $.fn.trackStar.defaults = {
        xOffset: 10,
        yOffset: 30
    };
})(jQuery);
