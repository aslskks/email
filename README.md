To send emails with Python, you primarily use the built-in smtplib library to interact with an SMTP server and the email package to construct the message content. 
Prerequisites
An email account with SMTP credentials (username and password).
The SMTP server address and port number for your email provider (e.g., for Gmail: smtp.gmail.com, port 465 for SSL or 587 for TLS). 
Example: Sending a Plain-Text Email (using SSL)
The following script connects to the email provider's server using a secure SSL connection, logs in, and sends the email. 
python
import smtplib
import ssl
from email.message import EmailMessage
import getpass # To securely ask for the password in the terminal

# --- Configuration ---
sender_email = "your_email@gmail.com"  # Replace with your email
receiver_email = "recipient_email@example.com" # Replace with the recipient's email
password = getpass.getpass("Enter your email password: ") # Prompt for password securely
smtp_server = "smtp.gmail.com"
port = 465  # For SSL

# --- Create the email message ---
msg = EmailMessage()
msg.set_content("This is the body of the email.") # The plain text content
msg['Subject'] = "A test email from Python"
msg['From'] = sender_email
msg['To'] = receiver_email

# --- Send the email ---
try:
    # Create a secure SSL context
    context = ssl.create_default_context()

    with smtplib.SMTP_SSL(smtp_server, port, context=context) as server:
        server.login(sender_email, password)
        server.send_message(msg)
    print("Email sent successfully!")

except smtplib.SMTPAuthenticationError:
    print("Authentication error. Check your email and password, or App Password settings if using Gmail.")
except Exception as e:
    print(f"An error occurred: {e}")
ssl.create_default_context(): This is highly recommended as it loads the system's trusted certificates and handles security protocols, ensuring a secure connection.
Security Note: It is best practice to use environment variables or a secrets manager to store sensitive information like passwords, rather than hardcoding them in the script. 
