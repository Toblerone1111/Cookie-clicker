import tkinter as tk

class CookieClicker:
    def __init__(self, root):
        self.root = root
        self.root.title("Cookie Clicker")

        self.cookie_count = 0
        self.cookies_per_click = 1
        self.upgrade_cost = 10

        self.shop_open = False

        self.cookie_label = tk.Label(root, text=f"Cookies: {self.cookie_count}", font=("Arial", 16))
        self.cookie_label.pack(pady=10)

        self.cookie_button = tk.Button(root, text="Click Me!", font=("Arial", 16), command=self.click_cookie)
        self.cookie_button.pack(pady=10)

        self.upgrade_button = tk.Button(root, text=f"Upgrade (+1 per click) - Cost: {self.upgrade_cost}", font=("Arial", 12), command=self.buy_upgrade)
        self.upgrade_button.pack(pady=10)

        self.shop_button = tk.Button(root, text="Toggle Shop", font=("Arial", 12), command=self.toggle_shop)
        self.shop_button.pack(pady=10)

        self.shop_frame = tk.Frame(root)
        self.shop_frame.pack(pady=10)

        self.shop_items = [
            {"name": "Golden Cookie", "cost": 50, "clicks": 5},
            {"name": "Copper Cookie", "cost": 25, "clicks": 2},
            {"name": "Diamond Cookie", "cost": 200, "clicks": 20},
            {"name": "Platinum Cookie", "cost": 500, "clicks": 50},
        ]

        self.shop_widgets = []
        for item in self.shop_items:
            button = tk.Button(
                self.shop_frame,
                text=f"{item['name']} - Cost: {item['cost']} (Adds {item['clicks']} clicks)",
                font=("Arial", 12),
                command=lambda i=item: self.buy_cookie(i)
            )
            self.shop_widgets.append(button)

    def click_cookie(self):
        self.cookie_count += self.cookies_per_click
        self.update_ui()

    def buy_upgrade(self):
        if self.cookie_count >= self.upgrade_cost:
            self.cookie_count -= self.upgrade_cost
            self.cookies_per_click += 1
            self.upgrade_cost = max(1, int(self.upgrade_cost * 1.5))
            self.update_ui()

    def toggle_shop(self):
        self.shop_open = not self.shop_open
        if self.shop_open:
            for widget in self.shop_widgets:
                widget.pack(pady=5)
        else:
            for widget in self.shop_widgets:
                widget.pack_forget()

    def buy_cookie(self, item):
        if self.cookie_count >= item['cost']:
            self.cookie_count -= item['cost']
            self.cookies_per_click += item['clicks']
            self.update_ui()

    def update_ui(self):
        self.cookie_label.config(text=f"Cookies: {self.cookie_count}")
        self.upgrade_button.config(text=f"Upgrade (+1 per click) - Cost: {self.upgrade_cost}")

if __name__ == "__main__":
    root = tk.Tk()
    game = CookieClicker(root)
    root.mainloop()
