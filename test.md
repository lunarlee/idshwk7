from PIL import Image
def mod(x,y):
    return x%y
def toasc(strr):
    return int(strr, 2)
def plus(string1): 
    #Python zfill() 方法返回指定长度的字符串，原字符串右对齐，前面填充0。
    return string1.zfill(8)
def get_key(strr):
    string1=""
    for i in range(len(strr)):
        string1=string1+""+plus(bin(ord(strr[i])).replace('0b',''))
    return string1
###
def encode(str1,str2,str3): 
    im = Image.open(str1) 
    #获取图片的宽和高
    width,height= im.size[0],im.size[1]
    key = get_key(str2) 
    keylen = len(key)
    count = 0
    for h in range(height):
        for w in range(width):
            pixel = im.getpixel((w,h))
            a=pixel[0]
            b=pixel[1]
            c=pixel[2]
            if count == keylen:
                break
            #隐藏信息
            a= a-mod(a,2)+int(key[count])
            im.putpixel((w,h),(a,b,c)) 
            count+=1
        if count == keylen:
            break
    im.save(str3)
###
def decode(str1,str2): 
    b="" 
    im = Image.open(str2)
    lenth = str1*8 
    width,height = im.size[0],im.size[1]
    count = 0
    for h in range(height): 
        for w in range(width):
            #获得(w,h)点像素的值
            pixel = im.getpixel((w, h))
            if count ==lenth:
                break
            count+=1
            b=b+str(mod(int(pixel[0]),2))
            
        if count == lenth:
            break
    result_str=""
    for i in range(0,len(b),8):        
        stra = toasc(b[i:i+8]) 
        result_str+=chr(stra)
    print(result_str)
if __name__ == '__main__':
    #原始图片
    str1="test.png"
    #嵌入字符串
    str2="57119312lilu"
    #保存的文件
    str3="save.png"
    #提取的字符长度
    str1_de=len(str2)
    #提取的新信息文件
    str2_de="save.png"

    encode(str1,str2,str3)
    decode(str1_de,str2_de)
