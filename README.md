# mosonry-imagesLoaded-Demo
预览效果----> [瀑布流简单demo---mosonry+imagesLoaded-Demo](https://fairyly.github.io/mosonry-imagesLoaded-Demo/)



#还有一种是`InfiniteScroll + Masonry + ImagesLoaded`这个就是无限循环加载的
```javascript
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>瀑布流（masonry+imagesloaded+infinitescroll）</title>
    <meta name="viewport" content="width=device-width,initial-scale=1, minimum-scale=1, maximum-scale=1">
    <meta name="apple-mobile-web-app-title" content="瀑布流">
    <script src="js/jquery-2.2.3.min.js"></script>
   <script type="text/javascript">
        var docEl = document.documentElement,
            //总来的来就是监听当然窗口的变化，一旦有变化就需要重新设置根字体的值
            resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
            recalc = function() {
                //设置根字体大小
                docEl.style.fontSize = 10 * (docEl.clientWidth / 320) + 'px';
            };
        //绑定浏览器缩放与加载时间
        window.addEventListener(resizeEvt, recalc, false);
        document.addEventListener('DOMContentLoaded', recalc, false);
    </script>
    <style type="text/css">
    .container{
        width: 93%;
        margin: 0px auto;
        margin-left: 1.5rem;
    }
    #images{
        width: 100%;
        margin:0px  auto;
    }
    .items {
        width: 46.5%;
        margin: 0 8px 14px 0;
        text-align:center;
        padding: 10px;
        float: left;  
        /*display: none; */
        border: 1px solid #DDD;
        background: #EEE;
        -moz-box-shadow:0 1px 3px rgba(0, 0, 0, .3);
        -webkil-box-shadow:0 1px 3px rgba(0, 0, 0, .3);
        box-shadow:0 1px 3px rgba(0, 0, 0, .3);
        box-sizing:border-box;
        -webkit-box-sizing:border-box;
    
    }
    .items img {
        width: 95%;
    }
</style>
</head>
<body>
    <div class="container">
        <div id="images">
            <div class="items"><img src="images/1.jpg"><p>描述文字</p></div>
            <div class="items"><img src="images/5.jpg"><p>描述文字</p></div>
            <div class="items"><img src="images/3.jpg"><p>描述文字</p></div>
            <div class="items"><img src="images/6.jpg"><p>描述文字</p></div>
            
            <div class="loading">
          
            </div>
            <div class="navigation">
                <a href="json/more1.json?page=1"></a>
            </div>
        </div>
    </div>

     <!-- 更多模板页面结束 -->
     <script type="text/javascript" src="js/masonry.pkgd.min.js"></script>
     <script type="text/javascript" src="js/imagesloaded.pkgd.min.js"></script>
     <script type="text/javascript" src="js/jquery.infinitescroll.min.js"></script>
     <script type="text/javascript">
        $(document).ready(
            function(){
                var $container = $('#images');
                $container.imagesLoaded(function(){
                    $container.masonry({
                        itemSelector : '.items',
                        isAnimated: true,
                    });
                }); 
                $container.infinitescroll({
                    navSelector  : ".navigation",
                    nextSelector : ".navigation a",
                    itemSelector : ".items",
                    dataType: 'json',
                    debug: true, 
                    animate: true,
                    loading: {
                        finishedMsg: 'No more pages to load.',
                        img: 'http://i.imgur.com/6RMhx.gif',
                        selector: '.loading'
                    },
                    template: function(data) {   
                               //data表示服务端返回的json格式数据，这里需要把data转换成瀑布流块的html格式，然后返回给回到函数  
                               var counter = 0;
                                // 每页展示4个
                                var num = 4;
                                var pageStart = 0,pageEnd = 0; 
                                var result = '';
                                counter++;
                                pageEnd = num * counter;
                                pageStart = pageEnd - num;
                                console.log("ajax请求成功");
                                for(var i = pageStart; i < pageEnd; i++){
                                    result += '<div class="items">'+'<img src="'+data.lists[i].pic+'" alt="">'+'<p>描述文字</p></div>';
                                }
                               console.log(data);
                               return result;   
                     },
                },function( newElements ) {
                        var $newElems = $( newElements ).hide();
                        var $newElems = $( newElements ).css({ opacity: 0 });
                        $newElems.imagesLoaded(function(){
                            $newElems.show();
                            $newElems.animate({ opacity: 1 });
                            $container.masonry( 'appended', $newElems, true );
                        });
                });
            }
        );
     </script>
```
