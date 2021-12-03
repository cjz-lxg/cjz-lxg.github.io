hello world 
#include <stdio.h>
#include<string.h>
#pragma warning(disable:4996)
int main() {
	/*1.输入
	2.将有效位单独先存起来,注意1.23400后面的00估计也算
	3.计算E后面的值,打0*/
	char temp[10000];
	scanf("%s", temp);
	char zhengfu;
	char youxiaowei[10000];
	char put0[10000] = { 0 };
	if (temp[0] == '-') {
		printf("-");
	}

	int cnt;
	for ( cnt = 1; cnt < strlen(temp); cnt++) {
		if (temp[cnt] == '.') continue;
		if (temp[cnt] == 'E') {
			youxiaowei[cnt] = 0;
			break;
		}
		youxiaowei[cnt] = temp[cnt];
	}
	
	zhengfu = temp[++cnt];
	for (++cnt; cnt < strlen(temp); cnt++) {
		put0[cnt] = temp[cnt];
	}
	put0[cnt] = 0;
	int length = strlen(put0);
	
	if (zhengfu == '-' ){
		for (int cnt = 0; cnt < length; cnt++) {
			printf("0");
			if (cnt == 1) {
				printf(".");
			}
		}
		for (int cnt = 0; cnt < strlen(youxiaowei); cnt++) {
			printf("%c", youxiaowei[cnt]);
		}
	}
	
	if (zhengfu == '+') {
		for (int cnt = 0; cnt < strlen(youxiaowei); cnt++) {
			printf("%c", youxiaowei[cnt]);
		}
		for (int cnt = 0; cnt < length; cnt++) {
			printf("0");
			if (cnt == 1) {
				printf(".");
			}
		}
	}
	
	return 0;


}失败代码

二次失败代码#include <stdio.h>
#include<string.h>
#pragma warning(disable:4996)
int main() {
	/*1.输入
	2.将有效位单独先存起来,注意1.23400后面的00估计也算
	3.计算E后面的值,打0*/
	char temp[10000];
	scanf("%s", temp);
	char zhengfu;
	char youxiaowei[10000];
	char put0[10000] = { 0 };
	if (temp[0] == '-') {
		printf("-");
	}

	int cnt;	
	for (cnt = 1; temp[cnt] != 'E'; cnt++) {
	}
	strncpy(youxiaowei, temp+1, cnt-1 );
	youxiaowei[cnt] = 0;
	
	zhengfu = temp[++cnt];
	for (++cnt; cnt < strlen(temp); cnt++) {
		put0[cnt] = temp[cnt];
	}
	put0[cnt+1] = 0;
	int length = strlen(put0);
	
	if (zhengfu == '-' ){
		for (int cnt = 0; cnt < length; cnt++) {
			printf("0");
			if (cnt == 1) {
				printf(".");
			}
		}
		for (int cnt = 0; cnt < strlen(youxiaowei); cnt++) {
			printf("%c", youxiaowei[cnt]);
		}
	}
	
	if (zhengfu == '+') {
		printf("%s", youxiaowei);
		for (int cnt = 0; cnt < length; cnt++) {
			printf("0");
			if (cnt == 1) {
				printf(".");
			}
		}
	}
	
	return 0;


}



踩过的坑:

1. strncpy函数的最后一个参数表示从第二个参数里复制多少个字符,将其赋值到第一个参数后不会在 第一个参数的后面加0,所以你得手动加,而也有很强的对应关系,第三个参数本身就是应该添加的0的下标位置;

2. 开始把小数点放在了前面,出现了两个0,就00.0……….

3. ```
   #include<stdio.h>
   #include<string.h>
   #pragma warning(disable:4996)
   #include<math.h>
   int main() {
   	char temp[10000];
   	scanf("%s", temp);
   
   	if (temp[0] == '-') {
   		printf("-");
   	}
   
   	char youxiaowei[10000];
   	int cnt;
   	for ( cnt = 1; temp[cnt] != 'E'; cnt++);
   	strncpy(youxiaowei, temp+1 , cnt-1);
   	youxiaowei[cnt-1] = 0;
   
   	cnt++;
   	char fuhao;
   	fuhao = temp[cnt++];
   
   	char put0[10000];
   	strcpy(put0, temp+cnt);
   	int e = atoi(put0);
   
   	if (fuhao == '+' && e>strlen(youxiaowei)-2 ) {
   		for (int cnt = 0; cnt < strlen(youxiaowei); cnt++) {
   			if (youxiaowei[cnt] != '.' ) {
   				putchar(youxiaowei[cnt]);
   			}
   		}
   		for (int cnt = 1; cnt < e; cnt++) {
   			putchar('0');
   		}
   
   	}
   	else if(fuhao == '+' && e < strlen(youxiaowei) - 2){
   		for (int cnt = 1; cnt < e ; cnt++) {
   			youxiaowei[cnt] = youxiaowei[cnt + 1];
   			youxiaowei[cnt + 1] = '.';
   		}
   		printf("%s", youxiaowei);
   	}
   
   	if (fuhao == '-' && e>=1) {
   		for (int cnt = 0; cnt < e; cnt++) {
   			if (cnt == 1) {
   				putchar('.');
   			}
   			putchar('0');
   		}
   		for (int cnt = 0; cnt < strlen(youxiaowei); cnt++) {
   			if (youxiaowei[cnt] != '.') {
   				putchar(youxiaowei[cnt]);
   			}
   		}
   	}else if(fuhao == '-' && e < 1){
   		printf("%s", youxiaowei);
   	}
   
   	return 0;
   
   }
   ```

   上面代码也有
