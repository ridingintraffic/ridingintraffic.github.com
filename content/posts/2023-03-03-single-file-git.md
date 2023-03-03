---
date: 2023-03-03T00:00:00-05:00
title: 'git single file'
draft: false
categories: [ daily ]
tags : [ git ]
---
Sometimes you just need a single file from a git repo.   I know that this is not the typical way we use git, but sometimes you just need to do a thing.  
So I want a single file,  how do i do that?  

```
git archive --format=tar --remote=ssh://git@github.com:someuser/somefile.git{main} -- my_file_name |tar xf -
```

and like that you can snag one file from one repo  and get on with your life.  