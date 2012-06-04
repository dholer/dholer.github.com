---
layout: post
title: jekyll 问题汇总
category : ruby
tags : [ruby,rails,win7]
tagline : 
---
{% include JB/setup %}

没有看似简单的事，简单的开头后面往往都九曲十弯
为什么要用本地调试？其实应该反问自己，如果不用本地调试，将会多么麻烦，每每有改动想要调试着看看效果，都至少要 add, commit, push 三条命令上传到 github 服务器，说白了就两个字“麻烦”。
编译 Jekyll 编辑网站只需在根目录执行 jekyll 命令，下面的命令是 GitHub 更新网站所使用的默认指令。

	$ jekyll --pygments --safe
	
现在执行这条命令，就会将整个网站创建在目录 `_site` 下。
如果没有安装 Apache 等 Web服务器，还可以使用 Jekyll 的内置 Web服务器。
	
	$ jekyll --server --auto

默认在端口4000开启 Web服务器，访问 `http://localhost:4000` 即可。

###下面是一些在 jekyll 运行时可能会出现的一些问题及解决方案：
<hr>
> When a Liquid exception is thrown by a post, the error-handling cannot call self.name
	
在 `lib/jekyll/post.rb` 中补充上 name 即可。

> Liquid error: incompatible character encodings:UTF-8 and IBM437

这个问题是在 Windows 下出现的，英文博文没问题，中文博文就会报错，原因是你所使用的控制台并不能工作 UTF-8。

- MyGit 控制台解决方案

临时：在执行 jekyll 命令前，将当前控制台的代码格式转为 UTF-8:

	$ export LC_ALL=en_US.UTF-8
	$ export LANG=en_US.UTF-8
	$ jekyll --server --auto
	
永久：添加两对用户自定义的环境变量，`LC_ALL=en_US.UTF-8` 和`LANG=en_US.UTF-8`

- cmd 控制台解决方案

临时：在执行 jekyll 命令前，将当前控制台的代码格式转为 UTF-8:

	chcp 65001
	jekyll --server --auto
	
永久：将 chcp 命令和 jekyll 命令合并成一个 rake 任务，文件名就叫 rakefile，在 cmd 控制台执行 `rake runwindows` 即可。

	task :runwindows do
	    puts '* Changing the codepage'
	    `chcp 65001`
	    puts '* Running Jekyll'
	    `jekyll --server --auto`
	end

>Ruby 1.9 character encoding changes

octopress在windows下出现的字符集错误
	Solving UTF problem with Jekyll on Windows
	
> Ruby193/lib/ruby/gems/1.9.1/gems/jekyll-0.11.2/lib/jekyll/convertible.rb:29:in ‘read_yaml’: invalid byte sequence in GBK (ArgumentError)"

貌似是 jekyll 的一个 bug
将 convertible.rb 的第29行改为：

	self.content = File.read(File.join(base, name), :encoding => "utf-8")
	
windows下git bash中执行jekyll —server出错，编码问题

> Running the classifier…this could take a while

若要解释清楚此怪，还得从 Related posts 说起，这个相关博文的定位源于下面这个函数：

	# Calculate related posts.
	#
	# Returns [<Post>]
	def related_posts(posts)
	  return [] unless posts.size > 1
	  
	  if Jekyll.lsi
	    self.class.lsi ||= begin
	      puts &quot;Running the classifier... this could take a while.&quot;
	      lsi = Classifier::LSI.new
	      posts.each { |x| $stdout.print(&quot;.&quot;);$stdout.flush;lsi.add_item(x) }
	      puts &quot;&quot;
	      lsi
	    end
	
	    related = self.class.lsi.find_related(self.content, 11)
	    related - [self]
	  else
	    (posts - [self])[0..9]
	  end
	end

相信你已经发现了，这句"Running the classifier… this could take a while."就产自这里，分类器一直在工作，分出哪些是相关的博文，所以当你的博文数量很多的时候，这个分类的时间就会很长，可能永远处于Running状态（只限于本地调试的时间）。
注意上面紧接着出现了一个关键词 lsi ， Jekyll Configuration 里面有对其的描述 "Produces an index for related posts(产生相关博文的索引)"，至此就知道了只要把 lsi 功能关掉即可。
在 _config.yml 可以找到对 lsi 的设置，直接删掉即可，默认是 lsi:false

> Latent semantic indexing




