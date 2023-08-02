
## Filter
### 
 if (request.getSession().getAttribute("employee")!=null){
            log.info("用户已登录,用户id为:{}",request.getSession().getAttribute("employee"));
            filterChain.doFilter(request,response);
            return;
        }
        
        这段代码中 filterChain.doFilter(request,response);的作用,没有它会怎样

A: 在Java的Servlet中，FilterChain是用于调用一系列过滤器的一个对象，每个过滤器都可以进行一些特定的操作，例如检查用户的身份，记录请求日志，压缩响应等。doFilter(request, response)方法就是用来触发这个过滤器链的下一个过滤器的。如果所有的过滤器都已经被调用过了，那么这个方法将会调用原始的请求处理器（例如Servlet）。

在你提供的代码中，filterChain.doFilter(request,response);这一行是在完成一些身份检查之后，调用下一个过滤器或者原始的请求处理器。如果用户已经登录（也就是说request.getSession().getAttribute("employee")不为null），那么这个方法就会被调用。

如果你删除了这一行，那么当用户已经登录时，下一个过滤器或者原始的请求处理器将不会被调用，这意味着请求的处理在此结束，不会有任何响应返回给用户。这通常是不希望的，因为即使用户已经登录，你也希望能继续处理他们的请求，而不是直接结束。

即使目前只有这一个过滤器，filterChain.doFilter(request, response); 这行代码仍然是必需的。原因是这行代码不仅会触发下一个过滤器（如果存在的话），而且在所有过滤器都被处理完后，它也会调用原始的请求处理器（例如Servlet）。Servlet是实际处理请求和生成响应的组件。

如果你删除了 filterChain.doFilter(request, response); 这行代码，即使用户已经通过了登录验证，请求也无法进一步被处理，因为请求不会被传递给Servlet。这将导致用户没有收到任何响应，这通常是不希望发生的。所以，即使只有一个过滤器，这行代码也是必需的。





### 