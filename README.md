# buildmassemail_client
from email.message import EmailMessage # for drafting or composing or writing email message
import smtplib
import csv



def credentials():
    id = '' # your email here
    pw = '' # your password here


    return (id, pw)



'''
Setting Up connection to SMTP server: smtpObj = smtplib.SMTP('smtp.gmail.com', 587)
Verifying that connection is intact: smptObj.ehlo() ; smtpObj.starttls()
Logging in to email via Python smtpObj.login()
Using the Email Message Object  msg = EmailMessage()
sending an email smtpobj.send_message()



'''



try:
    smtpObj = smtplib.SMTP('smtp.gmail.com', 587) # set up a connection with SMTP server
    smtpObj.ehlo() # checks whether connection is ok. Will give error if connection is not ok
    smtpObj.starttls() # TLS - transport layer security, to ensure our messages go securely


    #credentials is a function written by us, to return id and password
    id = credentials()[0]
    pwd = credentials()[1]



    smtpObj.login(id,pwd)    # logged in to my id


    # I had to disable 2FA - 2 factor authentication
    # I had to allow low security access



    with open('all_emails.csv', 'r') as email_file: # reading from a csv file
        csv_reader = csv.DictReader(email_file)  # allowing to convert each row into a dictionary
        
        for row in csv_reader:
            msg = EmailMessage()
            msg["To"] = row["To"]
            msg["From"] = row["From"]
            msg["Subject"] = row["Subject"]
            msg.set_content(row["Content"])


            smtpObj.send_message(msg)
    
    smtpObj.quit()




    # msg = EmailMessage() # create email message object


    # msg["From"] = "Aneeq"
    # msg["To"] = "gafaja8236@tebyy.com"
    # msg["Subject"] = "testing"
    # msg.set_content("Hi How are you?")


    # smtpObj.send_message(msg) # it will send an email using the given email object


    # smtpObj.quit() # it will quit the current smtp connection


except Exception as err:
    print(err)

