
## game/arithmetic

生成算式，循环 20 次提问后显示统计结果。对于错误回答，程序会调整这部分算式值的生成概率

关于链表操作，程序里有下面一段程序不容易理解，作用是摘取链表中的节点

    /*
     * Find the penalty at position `value'; decrement its penalty and
     * delete it if it reaches 0; return the corresponding value.
     */
    for (pp = &penlist[op][operand]; (p = *pp) != NULL; pp = &p->next) {
        if (p->penalty > value) {
            value = p->value;
            penalty[op][operand]--;
            if (--(p->penalty) <= 0) {
                p = p->next;
                (void)free((char *)*pp);
                *pp = p;
            }
            return(value);
        }
        value -= p->penalty;
    }


### index

标准库函数 index, rindex， 用于搜索字符串中第一个匹配到的字符位置，返回 NULL 表示无法匹配

    #include <strings.h>
    char *index(const char *s, int c);
    char *rindex(const char *s, int c);

### getopt

    #include <unistd.h>
    int getopt(int argc, char * const argv[], const char *optstring);
    extern char *optarg;
    extern int optind, opterr, optopt;

optind 保存下一个要处理的参数的偏移量


### 事件响应

    (void)signal( SIGINT, intr );

参见 man 7 signal，SIGINT 表示用户中断（KeyboardInterrupt）时使用 intr 中断处理函数进行响应


### lint

参考 [manpage lint](http://www.unix.com/man-page/FreeBSD/1/lint)

### K&R C 函数声明（旧式）

    void
    main(argc, argv)
        int argc; char **argv;

ASCII C 的带类型的参数列表是 X3J11 从 C++ 中吸收过来的， 参考 [CHISTORY](http://cm.bell-labs.com/cm/cs/who/dmr/chist.html) 

## bin/mv

移动文件（可以多个）到目标目录，或移动一个文件到目标文件（fastcopy）

### chown 函数

       #include <unistd.h>
       int chown(const char *path, uid_t owner, gid_t group);
       int fchown(int fd, uid_t owner, gid_t group);
       int lchown(const char *path, uid_t owner, gid_t group);

### chmod 函数

       #include <sys/stat.h>
       int chmod(const char *path, mode_t mode);
       int fchmod(int fd, mode_t mode);

### 更改文件访问时间函数 

       #include <sys/time.h>
       int utimes(const char *filename, const struct timeval times[2]);

       struct timeval {
           long tv_sec;        /* seconds */
           long tv_usec;       /* microseconds */
       };


