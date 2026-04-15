# password-manager-python
password manager python.
1. Install dependencies: pip install cryptography
2. Create an empty json.json file in the root directory.

import json
import base64
import os
from cryptography.fernet import Fernet
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.kdf.pbkdf2 import PBKDF2HMAC

salt = b'\x19\x1e\xdf\xaa\x18\x85\xb0\x96\x1a\x12\x8e\x1c\x83\x82\x82\x32'

def make_cipher(pswrd):
   
	kdf = PBKDF2HMAC(algorithm=hashes.SHA256(), length=32, salt=salt, iterations=100000)
	key = base64.urlsafe_b64encode(kdf.derive(pswrd.encode()))
	return Fernet(key)

while True:
	pswrd = input("введите пароль:")
	if pswrd == ("123"):

		cipher = make_cipher(pswrd)

		try:
			with open('json.json', 'rb') as f:
				encrypted_data = f.read()
				data = json.loads(cipher.decrypt(encrypted_data))
		except:
			data = []    	

		break

	else:
		print("неверно!") 

while True:
	print("1. добавить!")
	print("2. посмотреть!")
	print("3. удалить!")
	print("4. выход!")
	vibor = int(input(":"))

	if vibor == 1:
		s = input("Введите сайт: ")
		l = input("Введите логин: ")
		p = input("Введите пароль: ")

		
		new_entry = {"site": s, "login": l, "password": p}
		
		data.append(new_entry)

		text_data = json.dumps(data).encode()
		encrypted_data = cipher.encrypt(text_data)
		with open('json.json', 'wb') as f:
			f.write(encrypted_data)
		print("Сохранено!")
	
	elif vibor == 2:
		if len(data) > 0:
			for i, entry in enumerate(data, 1):

				print(f"{i}. {entry['site']}")

			viborsaita = int(input(":"))

			if viborsaita <= len(data):

				vybranniy = data[viborsaita - 1]

				print(f"Сайт: {vybranniy['site']}")
				print(f"Логин: {vybranniy['login']}")
				print(f"Пароль: {vybranniy['password']}")
		else:
			print("сайтов нет!")
	elif vibor == 3:
		if len(data) > 0:
			for i, entry in enumerate(data, 1):

				print(f"{i}. {entry['site']}")

			viborsaita2 = int(input(":"))	

			if viborsaita2 <= len(data):

				vybranniy2 = data.pop(viborsaita2 - 1)

				text_data = json.dumps(data).encode()
				encrypted_data = cipher.encrypt(text_data)
				with open('json.json', 'wb') as f: f.write(encrypted_data)
				print(f"{vybranniy2['site']} удалён!")

			else:
				print("ошибка!")
				
	elif vibor == 4:
		exit()
