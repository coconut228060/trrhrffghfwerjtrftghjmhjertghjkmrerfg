import random
import tkinter as tk
from tkinter import messagebox

# Варіанти вибору та їх смайлики
CHOICES = {
    "Камінь": "🪨",
    "Ножиці": "✂️",
    "Папір": "📄"
}

# Статистика гри
user_score = 0
bot_score = 0

def determine_winner(user_choice, bot_choice):
    """Логіка визначення переможця"""
    if user_choice == bot_choice:
        return "Нічия!"
    
    # Умови виграшу гравця
    winning_conditions = {
        "Камінь": "Ножиці",
        "Ножиці": "Папір",
        "Папір": "Камінь"
    }
    
    if winning_conditions[user_choice] == bot_choice:
        return "Ви виграли! 🎉"
    else:
        return "Бот виграв! 🤖"

def play(user_choice):
    """Обробка ходу гравця"""
    global user_score, bot_score
    
    bot_choice = random.choice(list(CHOICES.keys()))
    result = determine_winner(user_choice, bot_choice)
    
    # Оновлення рахунку
    if "Ви виграли" in result:
        user_score += 1
    elif "Бот виграв" in result:
        bot_score += 1

    # Відображення вибору та результату
    label_user_choice.config(text=f"Ваш вибір:\n{CHOICES[user_choice]} {user_choice}")
    label_bot_choice.config(text=f"Вибір бота:\n{CHOICES[bot_choice]} {bot_choice}")
    label_result.config(text=result)
    
    # Оновлення таблиці рахунку
    label_score.config(text=f"Гравець: {user_score}  |  Бот: {bot_score}")

def reset_game():
    """Скидання рахунку"""
    global user_score, bot_score
    user_score = 0
    bot_score = 0
    label_user_choice.config(text="Ваш вибір:\n❓")
    label_bot_choice.config(text="Вибір бота:\n❓")
    label_result.config(text="Зробіть свій хід!")
    label_score.config(text="Гравець: 0  |  Бот: 0")

# --- Налаштування головного вікна ---
root = tk.Tk()
root.title("Камінь, Ножиці, Папір")
root.geometry("400x480")
root.resizable(False, False)
root.configure(bg="#f0f2f5")

# Заголовок
title_label = tk.Label(root, text="Камінь • Ножиці • Папір", font=("Helvetica", 16, "bold"), bg="#f0f2f5", fg="#333")
title_label.pack(pady=15)

# Рахунок
label_score = tk.Label(root, text="Гравець: 0  |  Бот: 0", font=("Helvetica", 14, "bold"), bg="#e1e8ed", fg="#1da1f2", width=30, height=2)
label_score.pack(pady=5)

# Фрейм для відображення вибору
frame_choices = tk.Frame(root, bg="#f0f2f5")
frame_choices.pack(pady=15)

label_user_choice = tk.Label(frame_choices, text="Ваш вибір:\n❓", font=("Helvetica", 12), bg="#ffffff", width=15, height=3, relief="groove")
label_user_choice.grid(row=0, column=0, padx=10)

label_bot_choice = tk.Label(frame_choices, text="Вибір бота:\n❓", font=("Helvetica", 12), bg="#ffffff", width=15, height=3, relief="groove")
label_bot_choice.grid(row=0, column=1, padx=10)

# Результат раунду
label_result = tk.Label(root, text="Оберіть предмет, щоб почати!", font=("Helvetica", 13, "bold"), bg="#f0f2f5", fg="#2b2b2b")
label_result.pack(pady=15)

# Фрейм для кнопок вибору
frame_buttons = tk.Frame(root, bg="#f0f2f5")
frame_buttons.pack(pady=10)

btn_rock = tk.Button(frame_buttons, text="🪨\nКамінь", font=("Helvetica", 11, "bold"), width=8, height=3, bg="#ffffff", command=lambda: play("Камінь"))
btn_rock.grid(row=0, column=0, padx=5)

btn_scissors = tk.Button(frame_buttons, text="✂️\nНожиці", font=("Helvetica", 11, "bold"), width=8, height=3, bg="#ffffff", command=lambda: play("Ножиці"))
btn_scissors.grid(row=0, column=1, padx=5)

btn_paper = tk.Button(frame_buttons, text="📄\nПапір", font=("Helvetica", 11, "bold"), width=8, height=3, bg="#ffffff", command=lambda: play("Папір"))
btn_paper.grid(row=0, column=2, padx=5)

# Кнопка скидання
btn_reset = tk.Button(root, text="Скинути рахунок 🔄", font=("Helvetica", 10), bg="#ff4d4d", fg="white", activebackground="#cc0000", command=reset_game)
btn_reset.pack(pady=15)

# Запуск програми
root.mainloop()
