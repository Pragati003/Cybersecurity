# Cybersecurity
End to end encryption using crypto
from Crypto.PublicKey import RSA
from Crypto.Cipher import PKCS1_OAEP

class User:
    def __init__(self, name):
        self.name = name
        self.private_key = RSA.generate(2048)
        self.public_key = self.private_key.publickey()

def encrypt_message(message, public_key):
    cipher = PKCS1_OAEP.new(public_key)
    cipher_text = cipher.encrypt(message.encode())
    return cipher_text

def decrypt_message(cipher_text, private_key):
    cipher = PKCS1_OAEP.new(private_key)
    decrypted_message = cipher.decrypt(cipher_text)
    return decrypted_message.decode()

def send_encrypted_message(sender, receiver, message):
    encrypted_message = encrypt_message(message, receiver.public_key)
    decrypted_message = decrypt_message(encrypted_message, receiver.private_key)

    print(f"{sender.name} sends an encrypted message to {receiver.name}:")
    print(f"Encrypted Message: {encrypted_message}")
    print(f"{receiver.name} decrypts the message: {decrypted_message}\n")


# Example Usage:
user1 = User("Alice")
user2 = User("Bob")

message1 = "Hello Bob, how are you?"
send_encrypted_message(user1, user2, message1)

message2 = "Hi Alice, I'm doing well. How about you?"
send_encrypted_message(user2, user1, message2)
