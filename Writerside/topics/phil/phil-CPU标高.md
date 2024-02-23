# phil-CPU标高

## 生产CPU标高问题排查

1. 查看标高进程
    ```Shell
    # 直接执行TOP命令
    top
    ```

2. 找到PID-TID
    ```Shell
    # 找到对应线程
    ps H -eo pid,tid,%cpu | grep <PID>
    ```

3. 线程ID转十六进制
    ```Shell
    # 线程ID转十六进制 
    printf '0x%x\n' <TID>
    ```

4. 查看具体程序信息
    ```Shell
    # 查看具体程序信息
    jstack <PID> | grep <十六进制的线程ID> -A 20
   
    ### jstack 可以查看线程信息
    ```
   
   