# Implementation of Affine Cipher in Python

# Extended Euclidean Algorithm for finding modular inverse
# eg: modinv(7, 26) = 15
def egcd(a, b):
	x,y, u,v = 0,1, 1,0
	while a != 0:
		q, r = b//a, b%a
		m, n = x-u*q, y-v*q
		b,a, x,y, u,v = a,r, u,v, m,n
	gcd = b
	return gcd, x, y

def modinv(a, m):
	gcd, x, y = egcd(a, m)
	if gcd != 1:
		return None # modular inverse does not exist
	else:
		return x % m


# affine cipher encrytion function
# returns the cipher text
def affine_encrypt(text, key):
	'''
	C = (a*P + b) % 26
	'''
	return ''.join([ chr((( key[0]*(ord(t) - ord('A')) + key[1] ) % 26)
				+ ord('A')) for t in text.upper().replace(' ', '') ])


# affine cipher decryption function
# returns original text
def affine_decrypt(cipher, key):
	'''
	P = (a^-1 * (C - b)) % 26
	'''
	return ''.join([ chr((( modinv(key[0], 26)*(ord(c) - ord('A') - key[1]))
					% 26) + ord('A')) for c in cipher ])


def generateKey(string, key):
    key = list(key)
    if len(string) == len(key):
        return(key)
    elif len(string) > len(key):
        for i in range(len(string) - len(key)):
            key.append(key[i % len(key)])
        return("" . join(key))
    else: 
        key = key[:len(string)]
        return(key)
    print(key)
    
     
# This function returns the
# encrypted text generated
# with the help of the key
def cipherText(string, key):
    cipher_text = []
    for i in range(len(string)):
        x = (ord(string[i]) + ord(key[i])) % 26
        x += ord('A')
        cipher_text.append(chr(x))
    return("" . join(cipher_text))
     
# This function decrypts the
# encrypted text and returns
# the original text
def originalText(cipher_text, key):
    orig_text = []
    for i in range(len(cipher_text)):
        x = (ord(cipher_text[i]) - ord(key[i]) + 26) % 26
        x += ord('A')
        orig_text.append(chr(x))
    return("" . join(orig_text))

# Python program implementing Image Steganography

# PIL module is used to extract
# pixels of image and modify it
from PIL import Image

# Convert encoding data into 8-bit binary
# form using ASCII value of characters
def genData(data):

        # list of binary codes
        # of given data
        newd = []

        for i in data:
            newd.append(format(ord(i), '08b'))
        return newd

# Pixels are modified according to the
# 8-bit binary data and finally returned
def modPix(pix, data):

    datalist = genData(data)
    lendata = len(datalist)
    imdata = iter(pix)

    for i in range(lendata):

        # Extracting 3 pixels at a time
        pix = [value for value in imdata.__next__()[:3] +
                                imdata.__next__()[:3] +
                                imdata.__next__()[:3]]

        # Pixel value should be made
        # odd for 1 and even for 0
        for j in range(0, 8):
            if (datalist[i][j] == '0' and pix[j]% 2 != 0):
                pix[j] -= 1

            elif (datalist[i][j] == '1' and pix[j] % 2 == 0):
                if(pix[j] != 0):
                    pix[j] -= 1
                else:
                    pix[j] += 1
                # pix[j] -= 1

        # Eighth pixel of every set tells
        # whether to stop ot read further.
        # 0 means keep reading; 1 means thec
        # message is over.
        if (i == lendata - 1):
            if (pix[-1] % 2 == 0):
                if(pix[-1] != 0):
                    pix[-1] -= 1
                else:
                    pix[-1] += 1

        else:
            if (pix[-1] % 2 != 0):
                pix[-1] -= 1

        pix = tuple(pix)
        yield pix[0:3]
        yield pix[3:6]
        yield pix[6:9]

def encode_enc(newimg, data):
    w = newimg.size[0]
    (x, y) = (0, 0)

    for pixel in modPix(newimg.getdata(), data):

        # Putting modified pixels in the new image
        newimg.putpixel((x, y), pixel)
        if (x == w - 1):
            x = 0
            y += 1
        else:
            x += 1

# Encode data into image
def encode(cipher_text):
    img = input("Enter image name(with extension) for stego-encoding: ")
    image = Image.open(img, 'r')

    data = cipher_text
    if (len(data) == 0):
        raise ValueError('Data is empty')

    newimg = image.copy()
    encode_enc(newimg, data)

    new_img_name = input("Enter the name of new image(with extension) : ")
    newimg.save(new_img_name, str(new_img_name.split(".")[1].upper()))

# Decode the data in the image
def decode():
    img = input("Enter image name(with extension) for stego-decoding : ")
    image = Image.open(img, 'r')

    data = ''
    imgdata = iter(image.getdata())

    while (True):
        pixels = [value for value in imgdata.__next__()[:3] +
                                imgdata.__next__()[:3] +
                                imgdata.__next__()[:3]]

        # string of binary data
        binstr = ''

        for i in pixels[:8]:
            if (i % 2 == 0):
                binstr += '0'
            else:
                binstr += '1'

        data += chr(int(binstr, 2))
        if (pixels[-1] % 2 != 0):
            return data 


    
def main():

    a = int(input(":: Welcome to Steganography ::\n1. Encode\n2. Decode\n3. exit\n"))
    if( a== 1): 
        text = input('Enter plain text:\n')
        key = [17,20]
    
    # calling affine encryption function
        affine_encrypted_text = affine_encrypt(text,key)
        print('affine encrypted Text: {}\n'.format( affine_encrypted_text ))
    
    # calling ceaser encryption function
        string = affine_encrypted_text
        keyword = input("enter the public key\n")
        key2 = generateKey(string, keyword)
        ceaser_encrypted_text = cipherText(string,key2)
        print('private key is :')
        print(key2)
        print("ceaser encrypted text : ",ceaser_encrypted_text)
    
    # calling steganographic functions
        encode(ceaser_encrypted_text)
        main()

    elif( a== 2):
        key = [17,20]
        key2 = input('Enter private key\n')
        stegoDecrypt = decode()
        print("\nDecoded Word :  " + stegoDecrypt)
        # calling ceaser decryption function
        ceaserDecrypt = originalText(stegoDecrypt, key2)
        print("Ceaser Decrypted Text : ",ceaserDecrypt)
    
    # calling affine decryption function
        print('Original/ Affine decrypted Text: {}'.format( affine_decrypt(ceaserDecrypt, key) ))
        main()
        
    else:
        exit(0)
if __name__ == '__main__':
	main()
