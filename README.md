# Projekt-UVP

from tkinter import *
import time


class Nakupi():
    def __init__(self, master):
    
        self.s = 0
        self.sez = []
        
        master.title("Vnasanje nakupov")
        
        self.znesek = DoubleVar(master, value = 0)        
        Label(master, text="Znesek: ").grid(row=2, column=0)
        polje_znesek = Entry(master, textvariable=self.znesek)        
        polje_znesek.grid(row=2, column=1)

        self.nacin = StringVar(master, value="Gotovina")        
        Label(master, text="Nacin: ").grid(row=3, column=0)
        polje_nacin = OptionMenu(master,self.nacin, "Kartica")
        polje_nacin.grid(row=3, column=1)

        self.opomba = StringVar(master, value=" ")        
        Label(master, text="Opomba: ").grid(row=4, column=0)
        polje_opomba = Entry(master, textvariable=self.opomba)        
        polje_opomba.grid(row=4, column=1)
        
    
        self.gumb1 = Button(text="Dodaj", command=self.dodaj)
        self.gumb1.grid(row=6, column=0)        

        self.gumb2 = Button(text = "Popravek", command=self.popravi)
        self.gumb2.grid(row = 6, column=1)

        self.gumb3 = Button(text="Koncaj", command=self.koncaj)
        self.gumb3.grid(row=7, column=0)


        self.listbox = Listbox(master)
        self.listbox.grid(row=0, column=2, rowspan=8, columnspan=10)

        datoteka = open(str(time.strftime("%d.%m.%Y")) + ".txt", 'w')

    def dodaj(self):
            
        if self.znesek.get() >= 0:
            
            self.s += 1
            self.sez.append(self.znesek.get())
                
            vstavi = self.s,".", self.znesek.get(), "€,", self.nacin.get(), "," ,self.opomba.get()
            self.listbox.insert(END, vstavi)

            with open(str(time.strftime("%d.%m.%Y")) + ".txt", 'a', encoding = 'utf8') as datoteka:
                datoteka.write(str(self.s) + '.  ' + str(self.znesek.get()) + '€, ' + str(self.nacin.get()) + ', Opomba: ' + str(self.opomba.get()) + '\n')         

            self.nacin.set("Gotovina")             
            self.opomba.set(' ')
            self.znesek.set(0)

        
    def popravi(self):
        if self.s != 0:
            
            self.listbox.delete(END)
            self.s -= 1
            del self.sez[-1]
        
            lines = open(str(time.strftime("%d.%m.%Y")) + ".txt", 'r').readlines() 
            del lines[-1]  
            open(str(time.strftime("%d.%m.%Y")) + ".txt", 'w').writelines(lines) 
               

    def koncaj(self):
        self.gumb1.destroy()
        self.gumb2.destroy()
        self.gumb3.destroy()
        self.listbox.insert(END,"Dan zakljucen." )
        self.listbox.insert(END,"Podatki o poslovanju so shranjeni v datoteki z danasnjim datumom.")
        with open(str(time.strftime("%d.%m.%Y")) + ".txt", 'a', encoding = 'utf8') as datoteka:
            datoteka.write("\n St. opravljenih nakupov: " + str(self.s) + '\n Danes zasluzeno: ' + str(format(sum(self.sez), '.2f'))+ " EUR \n \n Zakljuceno ob: " + str(time.strftime("%H:%M:%S")))


root = Tk() 
aplikacija = Nakupi(root) 
root.mainloop()

