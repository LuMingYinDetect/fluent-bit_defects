Affected Version:
fluent-bit 2.2.2 9c3978cdaf932ca3756bd7d015c978d016544b04

Vulnerability Description:
The flaw is a double-free vulnerability occurring in line 762 of the read_config function in the file /fluent-bit/src/config_format/flb_cf_fluentbit.c. This vulnerability could be exploited, leading to program crashes and denial of service.

fluent-bit download address:
https://github.com/fluent/fluent-bit.git

Vulnerability details:

1.At line 419 of the file /fluent-bit/src/config_format/flb_cf_fluentbit.c, a pointer variable named buf is defined, as shown below:

![image](https://github.com/LuMingYinDetect/fluent-bit_defects/blob/main/fluent-bit_4.png)

2.The dynamic memory space pointed to by the pointer buf is released by passing it to the flb_free function at line 740. If the conditional statement at line 744 evaluates to true, the program will jump to the error label at line 747, as shown below:

![image](https://github.com/LuMingYinDetect/fluent-bit_defects/blob/main/fluent-bit_5.png)

3.In the flb_free function, only the free function is called to release the memory space pointed to by the corresponding pointer, without any additional operations being performed, as shown in the following code snippet:

![image](https://github.com/LuMingYinDetect/fluent-bit_defects/blob/main/fluent-bit_6.png)

4.After the program jumps to the error label at line 755, the buf pointer is again passed to the flb_free function at line 762, resulting in a double-free vulnerability, as illustrated in the following code snippet:

![image](https://github.com/LuMingYinDetect/fluent-bit_defects/blob/main/fluent-bit_7.png)
