# Graphical-User-Interface-Correspondence-Analysis

    # Python 2
    import tkinter as tk
    import tkinter.filedialog
    from tkinter.filedialog import askopenfilename
    import numpy as np
    import matplotlib.pyplot as plt
    import warnings
    import prince


    # Python 3
    import tkinter as tk
    import tkinter.filedialog
    from tkinter.filedialog import askopenfilename
    import numpy as np
    import matplotlib.pyplot as plt
    import prince
    import pandas as pd
    from PIL import Image, ImageTk




class MyWindow:
    
    def __init__(self, parent):

        self.parent=parent
        self.filename = None
        self.filename1 = None
        self.filename2 = None
        self.df = None
        self.df1 = None
        self.df2 = None
        self.parent = parent
        self.gambar1 = Image.open(r'd:/BACK 3.jpg')
        self.gambar = ImageTk.PhotoImage(self.gambar1)
        self.label = tk.Label(parent, image=self.gambar)
        self.label.pack()
        self.button = tk.Button(root, text='Load Data',bg='white', command=self.load).place(x=30,y=10,height=30)
        self.button = tk.Button(root, text='Load Nama Objek',bg='white', command=self.load1).place(x=30,y=50,height=30)
        self.button = tk.Button(root, text='Load Nama Variabel',bg='white', command=self.load2).place(x=30,y=90,height=30)
        self.button = tk.Button(root, text='Hasil dan Plot',bg='white', command=self.display).place(x=250,y=130,height=30)
        self.scrollbar= tk.Scrollbar(root)
        self.scrollbar.pack(side='right',fill='y')
        self.text = tk.Text(root, background="white")
        self.text.pack(expand=1, fill="both")
        self.text.config(yscrollcommand=self.scrollbar.set)
        self.scrollbar.config(command=self.text.yview)
        self.text5 = tk.Text(self.parent, height=2, width=30,bg='white')
        self.text5.place(x=200,y=10,height=30)
        self.text6 = tk.Text(self.parent, height=2, width=30,bg='white')
        self.text6.place(x=200,y=50,height=30)
        self.text7 = tk.Text(self.parent, height=2, width=30,bg='white')
        self.text7.place(x=200,y=90,height=30)
        self.tombol()
        self.fxn()

    def tombol(self):
        self.button = tk.Button(self.parent, text='Hapus File',bg='white', command=self.hapus).place(x=260,y=170,height=30)
    def load(self):

        name = askopenfilename(filetypes=[(('*.xls', '*.xlsx'))])

        if name:
            if name.endswith('.xlsx'):
                self.df = pd.read_excel(name,index_col=0)
            self.filename = name
        self.text5.insert('end', ((self.filename)))
    def load1(self):

        name1 = askopenfilename(filetypes=[(('*.xls', '*.xlsx'))])

        if name1:
            if name1.endswith('.xlsx'):
                self.df1 = pd.read_excel(name1, 'Sheet2',converters={'*':str})
            self.filename1 = name1
        self.text6.insert('end', ((self.filename1)))

    def load2(self):

        name2 = askopenfilename(filetypes=[(('*.xls', '*.xlsx'))])

        if name2:
            if name2.endswith('.xlsx'):
                self.df2 = pd.read_excel(name2, 'Sheet3',converters={'*':str})
            self.filename2 = name2
        self.text7.insert('end', ((self.filename1)))
    def display(self):
        if self.df is None:
            self.load()
        # display if loaded
        if self.df is not None:
            X = self.df
            ca = prince.CA(n_components=2,n_iter=3,copy=True,check_input=True,engine='auto',random_state=42)
            ca = ca.fit(X)
            coordinat=ca.column_coordinates(X)
            koordinat=ca.row_coordinates(X)
            koordinatx=koordinat.loc[:,0]
            koordinaty=koordinat.loc[:,1]
            coordinatx=coordinat.loc[:,0]
            coordinaty=coordinat.loc[:,1]
            self.text.tag_configure('big', font=('Verdana',10,'bold'), foreground= 'blue')
            self.text.insert('end', 'KOORDINAT VARIABEL\n', 'big', koordinat)
            self.text.insert('end', '\nKOORDINAT OBJEK\n', 'big', coordinat)
        if self.df1 is None:
            self.load1()
        if self.df2 is None:
            self.load2()
        if self.df1 is not None:
            if self.df2 is not None:
                NamaObjek=self.df1
                NamaVariabel=self.df2
                fig1, ax1 = plt.subplots()
                ax1.scatter(koordinatx,koordinaty)
                ax1.scatter(coordinatx,coordinaty)
                ax1.set_title('Plot Analisis Korespondensi')
                for (Objek1, _x1, _y1) in zip(NamaObjek, koordinatx, koordinaty):
                    ax1.annotate(Objek1, (_x1, _y1), color='green')
                for (Variabel1, _x2, _y2) in zip(NamaVariabel, coordinatx, coordinaty):
                    ax1.annotate(Variabel1, (_x2, _y2), color='blue')
                plt.axhline(y=0, xmin=0, xmax=1, linewidth=2, color='k')
                plt.axvline(x=0, ymin=0, ymax=1, linewidth=2, color='k')
        plt.show()
    def hapus(self):
            self.text.delete("1.0", "end")
            self.text5.delete("1.0", "end")
            self.text6.delete("1.0", "end")
            self.text7.delete("1.0", "end")
    def fxn(self):
            warnings.filterwarnings("ignore", category=DeprecationWarning)
            
if __name__ == '__main__':
    root = tk.Tk()
    root.resizable(0,0)
    root.title("ANALISIS KORESPONDENSI")
    root.geometry("600x500")
    root.iconbitmap(r'd:/unnamed_1_o7R_icon.ico')
    top = MyWindow(root)
    root.mainloop()
