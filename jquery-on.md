## jquery -----on方法usage-by GeenoLiT 
1.Event handlers are bound only to the currently selected elements; they must exist at the time 
your code makes the call to .on(). To ensure the elements are present and can be selected, 
place scripts after the elements in the HTML markup or perform event binding inside a document ready handler.
Alternatively, use delegated event handlers to attach event handlers.

>----适用于已经存在的元素绑定方式
$("元素").on("事件",function(){});


2.Delegated event handlers have the advantage that they can process events from descendant elements 
that are added to the document at a later time.

>----适用于，后来出现的元素，动态添加的元素
$("父元素","子元素","事件",function(){});
--比如列表里的行事件绑定
$( "#dataTable tbody" ).on( "click", "tr", function() {
  console.log( $( this ).text() );
});
