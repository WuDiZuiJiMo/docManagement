## 一、webpack 优化与 gzip 压缩

1. babel-loader 用 include 或者 exclude 来避免不必要的转译，不转移 node_modules 中的 js 文件，其次在缓存在转译的 js 文件，设置 loader:'babel-loader?cacheDirectory=true'
2. 文件采用按需加载
3. 在 request headers 中加上一句 accept-encoding:gzip
   启用 gzip 压缩功能，可以使网站的 css、js、xml、html 等静态资源在传输时进行压缩，经过 gzip 压缩后资源可以变为原来的 30%甚至更小，尽管这样会消耗一定的 cpu 资源，但是会大量的出口带宽来提高访问速度。
   gzip 的压缩页面需要浏览器和服务器双方都支持，实际上就是服务器端压缩，传到浏览器后解压并解析。浏览器哪里不需要我们担心，因为目前的大多数浏览器都支持 gzip。
   注意：不建议压缩图片和大文件
   图片如 jpg、png 文件本身就会压缩，所以就算开启 gzip 后，压缩前和压缩后大小没有多大区别，所以开启了反而会白白浪费 cpu 资源。而大文件资源会消耗大量的 cpu 资源，且不一定有明显的效果。
4. 图片优化，采用 svg 图片或字体图标
5. 浏览器缓存机制，又分为强缓存和协商缓存（有些重复数据可存入浏览器缓存，作为前端小数据库使用）

## 二、代码优化

1. 事件处理
2. 事件的防抖和节流
   防抖：事件触发 - 开启一个定时器 - 如果再次触发事件，清除定时器，重新开启一个 - 定时到触发操作
   节流：事件触发 - 执行操作&开启一个定时器关闭阀门 - 定时器时间内再次触发操作无效 - 定时器到，打开阀门 - 操作可再次触发
   相同点：都是为了高频操作触发，浪费性能。
   区别：防抖是触发高频事件 n 秒内只会执行一次。适用于可以多次触发但只生效最后一次的场景；
   节流是高频事件触发，但 n 秒内只会执行一次，如果 n 秒内多次触发只会执行第一次，节流会释放函数的执行频率。
3. 页面的回流和重绘
4. EventLoop 事件循环机制
5. for 和 if 分开使用
6. 资源懒加载和预加载
