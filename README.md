# attendance_monitoring_system
#include<reg51.h>
#include<string.h>
#define lcd P1
sbit rs=P3^6;
sbit e=P3^7;
sbit button=P3^3;
static char t=0,b=0,r=0,a=0;
void delay (int);
void cmd (unsigned char);
void display (unsigned char);
void string (char *);
void init (void);
void uart (void);
void list (void);
void list1 (char);
unsigned char rx (void);
void delay (int d)
{
unsigned char i=0;
for(;d>0;d--)
{
for(i=250;i>0;i--);
for(i=248;i>0;i--);
}
}
void cmd (unsigned char c)
{
lcd=c;
rs=0;
e=1;
delay(5);
e=0;
}
void display (unsigned char c)
{
lcd=c;
rs=1;
e=1;
delay(5);
e=0;
}
38
void string (char *p)
{
while(*p)
{
display(*p++);
}
}
void init (void)
{
cmd(0x38);
cmd(0x0c);
cmd(0x01);
cmd(0x80);
}
void uart (void)
{
TMOD=0x20;
SCON=0x50;
TH1=TL1=253;
TR1=1;
}
unsigned char rx (void)
{
while(RI == 0 && button == 1);
if(RI != 0)
{
RI = 0;
return SBUF;
}
else
list();
}
void list (void)
{
cmd(0x80);
string("Details..... ");
while(1)
{
cmd(0xc0);
string("Anusha - ");
list1(a);
delay(2000);
cmd(0xc0);
string("Jyoti - ");
list1(b);
delay(2000);
cmd(0xc0);
string("Nikhil - ");
list1(r);
delay(2000);
39
cmd(0xc0);
string("Palaksh - ");
list1(t);
delay(2000);
}
}
void list1 (char z)
{
if(z==1)
string("Present");
else
string("Absent ");
}
void main()
{
unsigned long int i=0;
unsigned char temp1[13],temp=0;
unsigned char palaksh[13]="13006F8C7282";
unsigned char nikhil[13]="13004993E32A";
unsigned char jyoti[13]="13006FF259D7";
unsigned char anusha[13]="13004A29E191";
P2=0x00;
init();
uart();
button=1;
cmd(0x01);
while(1)
{
cmd(0x80);
string("Swipe the card ");
for(i=0;i<12;i++)
{
temp1[i]=rx();
}
temp1[i]='\0';
if(strcmp(temp1,palaksh)==0)
{
cmd(0x80);
string("Welcome palaksh ");
t=1;
delay(2000);
}
40
else if(strcmp(temp1,nikhil)==0)
{
cmd(0x80);
string("Welcome nikhil ");
b=1;
delay(2000);
}
else if(strcmp(temp1,jyoti)==0)
{
cmd(0x80);
string("Welcome jyoti ");
r=1;
delay(2000);
}
else if(strcmp(temp1,anusha)==0)
{
cmd(0x80);
string("Welcome anusha ");
a=1;
delay(2000);
}
else
string("wrong card........");
}
}
