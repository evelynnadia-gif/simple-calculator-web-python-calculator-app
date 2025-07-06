import tkinter as tk
from tkinter import messagebox

class Calculator:
    def __init__(self, master):
        self.master = master
        master.title("Kalkulator Sederhana")
        master.geometry("300x400") # Lebar x Tinggi
        master.resizable(False, False) # Nonaktifkan resize
        master.configure(bg="#2c3e50") # Warna latar belakang

        self.expression = ""
        self.input_text = tk.StringVar()
        self.decimal_flag = False

        # Input / Layar Tampilan
        self.input_field = tk.Entry(master, textvariable=self.input_text, font=('Arial', 24, 'bold'),
                                    bd=10, insertwidth=4, width=14, justify='right', bg="#34495e", fg="white")
        self.input_field.grid(row=0, column=0, columnspan=4, padx=10, pady=10, sticky="nsew")

        # Tombol
        self.create_buttons()

    def create_buttons(self):
        buttons = [
            '7', '8', '9', '/',
            '4', '5', '6', '*',
            '1', '2', '3', '-',
            'C', '0', '.', '+',
            '='
        ]

        row_val = 1
        col_val = 0

        for button in buttons:
            if button == '=':
                btn = tk.Button(self.master, text=button, fg="white", bg="#2980b9",
                                font=('Arial', 18, 'bold'), height=2, width=6,
                                command=self.btn_equal)
                btn.grid(row=row_val, column=col_val, columnspan=4, sticky="nsew", padx=5, pady=5)
            elif button == 'C':
                btn = tk.Button(self.master, text=button, fg="white", bg="#e74c3c",
                                font=('Arial', 18, 'bold'), height=2, width=6,
                                command=self.btn_clear)
                btn.grid(row=row_val, column=col_val, sticky="nsew", padx=5, pady=5)
            else:
                btn = tk.Button(self.master, text=button, fg="white", bg="#34495e",
                                font=('Arial', 18, 'bold'), height=2, width=6,
                                command=lambda b=button: self.btn_click(b))
                btn.grid(row=row_val, column=col_val, sticky="nsew", padx=5, pady=5)

            col_val += 1
            if col_val > 3 and button != '=':
                col_val = 0
                row_val += 1

        # Mengatur berat kolom dan baris agar tombol rata
        for i in range(5): # 0-4 untuk baris
            self.master.grid_rowconfigure(i, weight=1)
        for i in range(4): # 0-3 untuk kolom
            self.master.grid_columnconfigure(i, weight=1)

    def btn_click(self, item):
        if item == '.':  # Manage decimal point
            if self.decimal_flag and '.' not in self.expression:
                self.expression += item
                self.input_text.set(self.expression)
            self.decimal_flag = True
        else:
            if not self.decimal_flag:
                self.expression += item
                self.input_text.set(self.expression)
            self.decimal_flag = False

    def btn_clear(self):
        self.expression = ""
        self.input_text.set("")

    def btn_equal(self):
        try:
            # Evaluasi ekspresi matematika
            result = str(eval(self.expression))
            self.input_text.set(result)
            self.expression = result # Agar hasil bisa dipakai untuk operasi selanjutnya
        except Exception as e:
            messagebox.showerror("Error", "Input Tidak Valid")
            self.expression = ""
            self.input_text.set("")

# Bagian Utama untuk Menjalankan Aplikasi
if __name__ == "__main__":
    root = tk.Tk()
    my_calculator = Calculator(root)
    root.mainloop()
