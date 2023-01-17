# devcontainer Zig template

vscode devcontainer template

<br><br><br><br>

Zig Language Server (zls)   
Formatter  
Debug

<br><br><br><br>


### X11

<br><br>

windows 10, 11 (wsl2 + wslg)  
ubuntu

<br>

mac  
Install XQuartz

<br><br><br>

Set a version you prefer.  

Dockerfile
```Dockerfile
ARG ZIGVER="0.10.0"
ARG ZIGTARGET="linux-x86_64"
```

<br><br><br><br><br>


## Hello world

<br>

```shell
zig init-exe
```

<br>

src/main.zig
```
const std = @import("std");

pub fn main() !void {
    std.debug.print("Hello, {s}!\n", .{"World"});
}
```

<br>

```shell
zig build
zig-out/bin/workspace
```



<br><br><br>

### F5

debug

<br><br><br>
<br><br><br>
<br><br><br>








