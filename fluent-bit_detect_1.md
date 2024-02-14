Affected Version:
fluent-bit 2.2.2 9c3978cdaf932ca3756bd7d015c978d016544b04

Vulnerability Description:
This vulnerability is a UAF (Use-After-Free) vulnerability discovered in the file /fluent-bit/plugins/custom_calyptia/calyptia.c. It could be maliciously exploited, leading to a denial-of-service attack.

fluent-bit download address:
https://github.com/fluent/fluent-bit.git

Vulnerability details:

1.On line 397 of the file /fluent-bit/plugins/custom_calyptia/calyptia.c, a pointer named ctx is defined, and dynamic memory is allocated for the pointer ctx on line 401. Subsequently, this pointer is passed as an argument to the setup_cloud_output function on line 455, as shown in the following image:

![image](https://github.com/LuMingYinDetect/fluent-bit_defects/blob/main/fluent-bit_1.png)

2.When the if condition statement on line 246 of the setup_cloud_output function is satisfied, the memory area pointed to by the pointer ctx is released on line 248, as shown in the following image:

![image](https://github.com/LuMingYinDetect/fluent-bit_defects/blob/main/fluent-bit_2.png)

3.After the setup_cloud_output function completes its execution and returns, the return value is assigned to ctx->o on line 455. However, the ctx pointer has already been released within the setup_cloud_output function, resulting in a use-after-free defect, as shown in the following image:

![image](https://github.com/LuMingYinDetect/fluent-bit_defects/blob/main/fluent-bit_3.png)
