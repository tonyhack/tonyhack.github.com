---
layout: post
category : vim
title: vim自动生成文件头
description: "用vim脚本快捷生成代码头"
tags : [vim, file title]
---

使用vim自动生成文件头只需要在.vimrc文件中增加以下代码即可：

{% highlight vim %}

map <F4> :call TitleDet()<CR>

function AddTitle()
  call append(0, "\/\/")
  call append(1, "\/\/ Summary: buzz source code.")
  call append(2, "\/\/")
  call append(3, "\/\/ Author: Tony.")
  call append(4, "\/\/ Email: tonyjobmails@gmail.com.")
  call append(5, "\/\/ Last modify: ".strftime("%Y-%m-%d %H:%M:%S"."."))
  call append(6, "\/\/ File name: ".expand("%:t"))
  call append(7, "\/\/")
  call append(8, "\/\/ Description:")
  call append(9, "\/\/")
  "echohl WarningMsg | echo "Successful in adding file title." | echohl None
endfunction

function UpdateTitle()
  normal m'
  execute '/ *Last modify:/s@:.*$@\=strftime(": %Y-%m-%d %H:%M:%S")."."@'
  normal ''
  normal mk
  execute '/ *File name:/s@:.*$@\=": ".expand("%:t")@'
  execute "noh"
  normal 'k
  "echohl WarningMsg | echo "Successful in updating file title." | echohl None
endfunction

function TitleDet()
  let n = 1
  while n < 10
    let line = getline(n)
    if line =~'\/\/\sLast\_smodify:'
      call UpdateTitle()
      return
    endif
    let n = n + 1
  endwhile
  call AddTitle()
endfunction

{% endhighlight %}

新建文件后，按F4生成文件名，每次需要更新时，也按F4，修改文件修改时间。
