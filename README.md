# MANDA CV 3000

## Preliminares

El propósito de este programa no es más que el de facilitar el envio de emails con un texto "plantilla" que además adjunte el Cv a las distintas empresas sin tener que:

* acceder a gmail, pegar la plantilla, modificar los datos acorde a la empresa que se dirija, adjuntar el cv y por último darle a enviar.

El programa está preparado para el envio desde Gmail, entiendase que para que esto pueda hacerse, se debe activar la opción de "perimitir usar aplicaciones menos segura". Además de tener como contenido de mensaje un ejemplo que a mi (autor) me sirve para mi proposito, pero que no deja de ser modificable (en el codigo está indicado)

## Instrucciones

1. Descargas el archivo de python «MandaCV3000.py» y la carpeta "data", lo colocas en el mismo directorio.
2. Dentro de la carpeta "data", añades el curriculum en pdf que quieras enviar con el nombre (por defecto) "cv.pdf"

3. Ejecutas el archivo .py

4. Escribe tu nombre, el nombre de la empresa a la que le vas a mandar el correo y su email

5. Le das a enviar

```python
# -*- coding: utf-8 -*-
import time
from email.mime.multipart import MIMEMultipart
from email.mime.application import MIMEApplication
from email.mime.text import MIMEText
import smtplib
from numpy import *
from matplotlib.pyplot import *
from tkinter import *

def enviar():
    msg = MIMEMultipart()
    #parametros
    password = "password" #aqui poner la contraseña
    msg['From'] = "miemail@gmail.com" #aqui cambiar el email de origen
    msg['To'] = email_cuadro.get()
    msg['Subject'] = 'Practicas/empleo'

    name= name_cuadro.get()
    empresa = empresa_cuadro.get()
    #Este va a ser el mensaje plantilla que se vaya a enviar por email
    msj = "Buenos dias, mi nombre es {0} y le escribo por el siguiente asunto:\n".format(name) +"\n" +"Soy graduado en Matemáticas por la universidad de Málaga. He visto su empresa {0}, me he informado acerca del sector al que pertenecéis, del trabajo que realizáis y estoy bastante interesado y entusiasmado en formar parte de ella. Me gustaría saber si disponéis de alguna vacante adecuada a mi formación o a los intereses que os pueda generar.".format(empresa)+"\n"+"\n"+"Gracias, un cordiar saludo" + "\n" +name
    mensaje = msj

    #añade el mensaje al cuerpo del correo
    msg.attach(MIMEText(mensaje,'plain'))
    #añade archivo al correo
    pdf = MIMEApplication(open("data/cv.pdf","rb").read()) #aquí busca y adjunta el archivo cv.pdf dentro de la carpeta data, si se quiere modificar la dirección tan sólo hay que modificar "data/cv.pdf" por la nueva dirección y el nombre del archivo con su extensión
    pdf.add_header('Content-Disposition', 'arrachment', filename="cv.pdf")
    msg.attach(pdf)

    #Crea servidor
    server = smtplib.SMTP(host='smtp.gmail.com', port=587)
    server.starttls()
    #Logueo con las credenciales
    server.login(msg['From'], password)
    #envia el mensaje via el servidor de email
    server.sendmail(msg['From'], msg['To'], msg.as_string())
    server.quit()
    print "Email enviado exitosamente a {0}".format(msg['To'])

raiz=Tk()
raiz.title('MandaCV3000')

#botones
name_etiqueta = Label(raiz,text='Nombre:')
name = StringVar()
name.set('')
name_cuadro=Entry(raiz,textvariable=name)

empresa_etiqueta = Label(raiz,text='Empresa:')
empresa = StringVar()
empresa.set('')
empresa_cuadro=Entry(raiz,textvariable=empresa)

email_etiqueta = Label(raiz,text='Email:')
email = StringVar()
email.set('')
email_cuadro=Entry(raiz,textvariable=email)

botonenviar = Button(raiz, text = 'Enviar', command = enviar)

#Distribucion en la ventana
name_etiqueta.grid(row=0)
name_cuadro.grid(row=0,column=1)
empresa_etiqueta.grid(row=1)
empresa_cuadro.grid(row=1,column=1)
email_etiqueta.grid(row=2)
email_cuadro.grid(row=2,column=1)
botonenviar.grid(row=3,column=1)
raiz.mainloop()

```

