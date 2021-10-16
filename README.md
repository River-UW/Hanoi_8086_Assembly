# Hanoi_8086_Assembly
<font size =4 face=Times New Roman>&emsp;&emsp;To solve Hanoi Tower Problem in 8086 assembly language.
</font>
 <br>
<font size =4 face=宋体>&emsp;&emsp;使用8086汇编语言解决汉诺塔问题
</font>
<br>

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://img-blog.csdnimg.cn/5887d2bb491b4a6f89545ced80b0321b.PNG">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图 1 程序运行示意（输入3）</div>
</center>
<br>

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://img-blog.csdnimg.cn/6eec8a1e8c4b46eca181a8c7eb8a6635.PNG">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图 2 程序运行示意（判断输入是否有效）</div>
</center>
<br>


<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://img-blog.csdnimg.cn/d583e5bbea034ea9923858e7296d2d6a.PNG">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图 3 程序运行示意（输入10）</div>
</center>
<br>

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://img-blog.csdnimg.cn/74276e6e3adf44dbaf5d13ae3893629a.PNG">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图 4 程序运行示意（控制台查看输入10的部分运行结果）</div>
</center>
<br>

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://img-blog.csdnimg.cn/9576aae63c2646d8b339b601e009ce99.PNG">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图 5 程序运行示意（输入10并将输出重定向至文件）</div>
</center>
<br>
<br>

<font size =4 face=宋体>&emsp;&emsp;**若在控制台无法查看完整运行结果，可以将其输出到文件后再行查看。**
</font>
<br>

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://img-blog.csdnimg.cn/1164be5a1d8c4992a946a5f19ce4b160.PNG">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图 6 查看重定向至文件的输入10的输出结果</div>
</center>
<br>


<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://img-blog.csdnimg.cn/1c366461df324f658836b12cb701f7c3.PNG">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图 7 程序运行示意（输入23并将输出重定向至文件）</div>
</center>
<br>

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://img-blog.csdnimg.cn/0ac1ec50940b4b7dae1ebf4785c33662.PNG">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图 8 查看重定向至文件的输入23的输出结果</div>
</center>
<br>

```assembly 
stack   segment stack
        db 512 dup(?)
stack   ends
    
data    segment
		string1 db "Enter a decimal number(Recommendation: 1~23)", 0ah, 0dh, '$'
        string2 db "Press any key to continue!", 0ah, 0dh, '$'
		string3 db "Invalid input, please enter a decimal number again:(Recommendation: 1~23)", 0ah, 0dh, '$'
		msg		db " --> ", '$'
data    ends
		
code 	segment
        assume ds: data, cs: code, ss: stack
main:
		mov ax, stack
        mov ss, ax
		mov ax, data
        mov ds, ax
		
		mov	al, 'A'
		push	ax
		mov	al, 'B'
		push	ax
		mov	al, 'C'
		push	ax
		;mov	ax, 8
		lea dx, string1
		mov ah, 09h
		int 21h
		
		call	far ptr readsiw
		
		mov bx, ax ;
		push bx ;
		jmp short judge_begin

input_again:
		lea dx, string3
		mov ah, 09h
		int 21h
		
		call	far ptr readsiw
		pop bx
		mov bx, ax ;
		push bx ;

judge_begin:
		
		call	far ptr judge_n ;
		cmp ax, 0
		je input_again
		
		pop bx
		mov ax, bx
		push	ax
		push	cs
		call	near ptr Hanoi
		
		jmp almost
		
; to ensure 1 <= n <= 23
judge_n	proc	far
		push	bp
		mov	bp, sp
		push	si
		mov	si, word ptr [bp+6]	
		cmp	si, 1
		jl	short invalie_n
		cmp	si, 23
		jg	short invalie_n
		mov	ax, 1
		jmp	short judge_finish
invalie_n:
		xor	ax, ax
judge_finish:
		jmp	short judge_done
judge_done:	
		pop	si
		pop	bp
		ret	
judge_n	endp

readsiw proc far
		push bx
		push cx
		push dx
		xor bx, bx   
		xor cx, cx   
		mov ah, 1
		int 21h
		cmp al, '+'
		jz rsiw1
		cmp al, '-'
		jnz rsiw2
		mov cx, -1
rsiw1: 
		mov ah,  1
		int 21h
rsiw2:    
		cmp al, '0'
		jb rsiw3
		cmp al, '9'
		ja rsiw3
		sub al, 30h
		xor ah, ah
		shl bx, 1
		mov dx, bx
		shl bx, 1
		shl bx, 1
		add bx, dx
		add bx, ax
		jmp rsiw1
rsiw3:
		cmp cx, 0
		jz rsiw4
		neg bx
rsiw4:
		mov ax, bx
		pop dx
		pop cx
		pop bx
		ret
readsiw endp

lineFeed proc far
		push ax
		push dx
		mov dl, 0dh
		mov ah, 2
		int 21h
		mov dl, 0ah
		mov ah, 2
		int 21h
		pop dx
		pop ax
		ret
lineFeed endp

move	proc	far
		push	bp
		mov	bp,sp
	
		mov	al,byte ptr [bp+8]
		cbw
		mov dl, al
		mov ah, 06h
		int 21h
		push	ax
		
		lea dx, msg
		mov ah, 09h
		int 21h
		
		mov	al,byte ptr [bp+6]
		cbw
		mov dl, al
		mov ah, 06h
		int 21h
		push	ax
		call	far ptr lineFeed

		add	sp, 4	
		jmp	short move_done
move_done:	
		pop	bp
		ret	
move	endp
	
	
Hanoi	proc	far
		push	bp
		mov	bp, sp
		push	si
		mov	si, word ptr [bp+6]
	
		cmp	si, 1
		jne	short greater
		
		mov	al, byte ptr [bp+12]
		push	ax
		mov	al, byte ptr [bp+8]
		push	ax
		push	cs
		call	near ptr move
		pop	cx
		pop	cx
	
		jmp	short to_finish
greater:	
		mov	al, byte ptr [bp+10]
		push	ax
		mov	al, byte ptr [bp+12]
		push	ax
		mov	al, byte ptr [bp+8]
		push	ax
		mov	ax, si
		dec	ax
		push	ax
		push	cs
		call	near ptr Hanoi
		add	sp, 8
	
		mov	al, byte ptr [bp+12]
		push	ax
		mov	al, byte ptr [bp+8]
		push	ax
		push	cs
		call	near ptr move
		pop	cx
		pop	cx
	
		mov	al, byte ptr [bp+12]
		push	ax
		mov	al, byte ptr [bp+8]
		push	ax
		mov	al, byte ptr [bp+10]
		push	ax
		mov	ax, si
		dec	ax
		push	ax
		push	cs
		call	near ptr Hanoi
		add	sp, 8
to_finish:	
		jmp	short hanoi_done
hanoi_done:	
		pop	si
		pop	bp
		ret	
Hanoi	endp

almost:
		lea dx, string2
		mov ah, 09h
		int 21h
		
		mov ah, 07h
		int 21h
		
		mov ah, 4ch
		int 21h
code	ends
		end main
```

