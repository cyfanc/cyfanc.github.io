---
title: Ebcdic To Ascii  
published: true
---

# Ebcdic To Ascii
## test.c
```c
#include <stdio.h>
#include <string.h>

char e2a[256] =
{
    0,  1,  2,  3,(char)156,  9,(char)134,127,(char)151,(char)141,(char)142, 11, 12, 13, 14, 15,
    16, 17, 18, 19,(char)157,(char)133,  8,(char)135, 24, 25,(char)146,(char)143, 28, 29, 30, 31,
    (char)128,(char)129,(char)130,(char)131,(char)132, 10, 23, 27,(char)136,(char)137,(char)138,(char)139,(char)140,  5,  6,  7,
    (char)144,(char)145, 22,(char)147,(char)148,(char)149,(char)150,  4,(char)152,(char)153,(char)154,(char)155, 20, 21,(char)158, 26,
    32,(char)160,(char)161,(char)162,(char)163,(char)164,(char)165,(char)166,(char)167,(char)168, 91, 46, 60, 40, 43, 33,
    38,(char)169,(char)170,(char)171,(char)172,(char)173,(char)174,(char)175,(char)176,(char)177, (char)93, 36, 42, 41, 59, 94,
    45, 47,(char)178,(char)179,(char)180,(char)181,(char)182,(char)183,(char)184,(char)185,(char)124, 44, 37, 95, 62, 63,
    (char)186,(char)187,(char)188,(char)189,(char)190,(char)191,(char)192,(char)193,(char)194, 96, 58, 35, 64, 39, 61, 34,
    (char)195, 97, 98, 99,(char)100,(char)101,(char)102,(char)103,(char)104,(char)105,(char)196,(char)197,(char)198,(char)199,(char)200,(char)201,
    (char)202,(char)106,107,108,109,110,111,112,113,114,(char)203,(char)204,(char)205,(char)206,(char)207,(char)208,
    (char)209,(char)126,115,116,117,118,119,(char)120,(char)121,(char)122,(char)210,(char)211,(char)212,(char)213,(char)214,(char)215,
    (char)216,(char)217,(char)218,(char)219,(char)220,(char)221,(char)222,(char)223,(char)224,(char)225,(char)226,(char)227,(char)228,
    (char)229,(char)230,(char)231,
    (char)123, 65, 66, 67, 68, 69, 70, 71, 72, 73,(char)232,(char)233,(char)234,(char)235,(char)236,(char)237,
    (char)125, 74, 75, 76, 77, 78, 79, 80, 81, 82,(char)238,(char)239,(char)240,(char)241,(char)242,(char)243,
    92,(char)159, 83, 84, 85, 86, 87, 88, 89, 90,(char)244,(char)245,(char)246,(char)247,(char)248,(char)249,
    48, 49, 50, 51, 52, 53, 54, 55, 56, 57,(char)250,(char)251,(char)252,(char)253,(char)254,(char)255
};

char a2e[256] =
{
    (char)0,  (char)1,  (char)2,  (char)3,  (char)55, (char)45, (char)46, (char)47, (char)22, (char)5, (char)37, (char)11, (char)12, (char)13, (char)14, (char)15,
    (char)16, (char)17, (char)18, (char)19, (char)60, (char)61, (char)50, (char)38, (char)24, (char)25, (char)63, (char)39, (char)28, (char)29, (char)30, (char)31,
    (char)64, (char)79, (char)127,(char)123,(char)91, (char)108,(char)80, (char)125, (char)77, (char)93, (char)92, (char)78,(char)107, (char)96, (char)75,(char)97,
    (char)240,(char)241,(char)242,(char)243,(char)244,(char)245,(char)246,(char)247,(char)248,(char)249,(char)122, (char)94, (char)76,(char)126,(char)110,(char)111,
    (char)124,(char)193,(char)194,(char)195,(char)196,(char)197,(char)198,(char)199,(char)200,(char)201,(char)209,(char)210,(char)211,(char)212,(char)213,(char)214,
    (char)215,(char)216,(char)217,(char)226,(char)227,(char)228,(char)229,(char)230,(char)231,(char)232,(char)233, (char)74,(char)224, (char)90, (char)95,(char)109,
    (char)121,(char)129,(char)130,(char)131,(char)132,(char)133,(char)134,(char)135,(char)136,(char)137,(char)145,(char)146,(char)147,(char)148,(char)149,(char)150,
    (char)151,(char)152,(char)153,(char)162,(char)163,(char)164,(char)165,(char)166,(char)167,(char)168,(char)169,(char)192,(char)106,(char)208,(char)161,  (char)7,
    (char)32, (char)33, (char)34, (char)35, (char)36, (char)21,  (char)6, (char)23, (char)40, (char)41, (char)42, (char)43, (char)44,  (char)9, (char)10,(char) 27,
    (char)48, (char)49, (char)26, (char)51, (char)52, (char)53,(char) 54,  (char)8, (char)56, (char)57, (char)58, (char)59,  (char)4, (char)20, (char)62,(char)225,
    (char)65, (char)66, (char)67, (char)68, (char)69, (char)70, (char)71, (char)72, (char)73, (char)81, (char)82, (char)83, (char)84, (char)85, (char)86,(char) 87,
    (char)88, (char)89, (char)98, (char)99,(char)100,(char)101,(char)102,(char)103,(char)104,(char)105,(char)112,(char)113,(char)114,(char)115,(char)116,(char)117,
    (char)118, (char)119,(char)120,(char)128,(char)138,(char)139,(char)140,(char)141,(char)142,(char)143,(char)144,(char)154,(char)155,(char)156,(char)157,(char)158,
    (char)159,(char)160,(char)170,(char)171,(char)172,(char)173,(char)174,(char)175,(char)176,(char)177,(char)178,(char)179,(char)180,(char)181,(char)182,(char)183,
    (char)184,(char)185,(char)186,(char)187,(char)188,(char)189,(char)190,(char)191,(char)202,(char)203,(char)204,(char)205,(char)206,(char)207,(char)218,(char)219,
    (char)220,(char)221,(char)222,(char)223,(char)234,(char)235,(char)236,(char)237,(char)238,(char)239,(char)250,(char)251,(char)252,(char)253,(char)254,(char)255
};

void EbcToAsk(char * in, char* out, int len)
{
    while(len)
    {
      *out++ = e2a[ (unsigned char)*in++ ];
      --len;
    }

    return ;
}

void AskToEbc(char* in, char* out, int len)
{
    int i = 0;
    while(i < len)
    {
      *out++ = a2e[  (unsigned char)*in++  ] ;
      --len;
    }

    return;
}

int main()
{
    char in[100] = "123456!@#$%^";
    char out[100] = "";

    AskToEbc(in,out,12);
    printf("in:%s\n", in);
    printf("out:%s\n", out);

    EbcToAsk(out,in,12);
    printf("in:%s\n", out);
    printf("out:%s\n", in);

    return 0;
}
```
