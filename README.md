# my-fourth-task
from PIL import Image
import numpy as np
import random

def encrypt_image(image_path, output_path):
    # Open the image
    img = Image.open(image_path)
    
    # Convert the image to a NumPy array
    img_array = np.array(img)
    
    # Generate a random key between 1 and 255
    key = random.randint(1, 255)
    
    # Encrypt the image by adding the key to each pixel, and wrap around using modulo 256
    # Convert the array to a larger type to avoid overflow before applying modulo
    encrypted_array = (img_array.astype(np.int16) + key) % 256
    
    # Convert the encrypted array back to an image
    encrypted_img = Image.fromarray(np.uint8(encrypted_array))
    
    # Save the encrypted image
    encrypted_img.save(output_path)
    print(f"Image encrypted and saved as {output_path}")
    print(f"Your encryption key is: {key}")
    
    return key

def decrypt_image(encrypted_image_path, key, output_path):
    # Open the encrypted image
    encrypted_img = Image.open(encrypted_image_path)
    
    # Convert the image to a NumPy array
    encrypted_array = np.array(encrypted_img)
    
    # Decrypt the image by subtracting the key from each pixel, and wrap around using modulo 256
    decrypted_array = (encrypted_array.astype(np.int16) - key) % 256
    
    # Convert the decrypted array back to an image
    decrypted_img = Image.fromarray(np.uint8(decrypted_array))
    
    # Save the decrypted image
    decrypted_img.save(output_path)
    print(f"Image decrypted and saved as {output_path}")

def main():
    while True:
        # Ask the user for encryption or decryption choice
        choice = input("Do you want to (E)ncrypt or (D)ecrypt an image? Type 'E' or 'D' (or 'exit' to quit): ").lower()

        if choice == 'e':
            # Encryption path
            image_path = input("Enter the path of the image to encrypt: ")
            output_path = input("Enter the path to save the encrypted image: ")
            key = encrypt_image(image_path, output_path)
            print(f"Encryption complete! Keep this key safe to decrypt the image: {key}")
        
        elif choice == 'd':
            # Decryption path
            encrypted_image_path = input("Enter the path of the encrypted image: ")
            try:
                key = int(input("Enter the encryption key: "))
            except ValueError:
                print("Invalid key! Please enter a valid number.")
                continue
            output_path = input("Enter the path to save the decrypted image: ")
            decrypt_image(encrypted_image_path, key, output_path)
            print("Decryption complete!")
        
        elif choice == 'exit':
            # Exit the loop
            print("Exiting the program.")
            break
        
        else:
            print("Invalid option! Please choose 'E' for encryption, 'D' for decryption, or 'exit' to quit.")

if __name__ == "__main__":
    main()
