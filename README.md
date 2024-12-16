from abc import ABC, abstractmethod


# абстрактный
class DepositType(ABC):
    @abstractmethod
    def calculate_deposit(self, deposit_size: float) -> float:
        pass

# дочерний
class StandardDeposit(DepositType):
    def calculate_deposit(self, deposit_size: float) -> float:
        return deposit_size 

#дочерний
class BonusDeposit(DepositType):
    def calculate_deposit(self, deposit_size: float) -> float:
        bonus = 1000  
        return deposit_size + bonus

# клиент
class Client:
    def __init__(self, name: str, num_of_deposits: int, deposit_size: float, deposit_type: DepositType):
        self.name = name
        self.num_of_deposits = num_of_deposits
        self.deposit_size = deposit_size
        self.deposit_type = deposit_type

    def client_info(self) -> None:
        total_deposit = self.deposit_type.calculate_deposit(self.deposit_size) * self.num_of_deposits
        print(f"Имя вкладчика: {self.name}")
        print(f"Количество вкладов: {self.num_of_deposits}")
        print(f"Размер одного вклада: {self.deposit_size}")
        print(f"Общая сумма вкладов: {total_deposit}")

# банк
class Bank:
    def __init__(self):
        self.clients = []

    def add_new_client(self, name: str, num_of_deposits: int, deposit_size: float, deposit_type: DepositType) -> None:
        new_client = Client(name, num_of_deposits, deposit_size, deposit_type)
        self.clients.append(new_client)
        print(f"Вкладчик {name} добавлен в банк.")

    def calculate_total_deposits(self) -> float:
        total_sum = 0
        for client in self.clients:
            total_sum += client.deposit_type.calculate_deposit(client.deposit_size) * client.num_of_deposits
        return total_sum

    def bank_info(self) -> None:
        if not self.clients:
            print("В банке нет клиентов.")
        else:
            for client in self.clients:
                client.client_info()
                print("---")


def menu():
    print("\n--- Меню ---")
    print("1. Добавить нового вкладчика")
    print("2. Показать информацию о всех вкладчиках")
    print("3. Вычислить общую сумму вкладов")
    print("4. Выйти")


def run_bank_system():
    bank = Bank()

    while True:
        menu()
        choice = input("Выберите опцию: ")
        print("---")

        if choice == "1":
         
            while True:
                name = input("Введите имя вкладчика: ").strip()
                if name.isalpha():
                    break
                else:
                    print("Имя должно содержать только буквы. Попробуйте снова.")

         
            while True:
                try:
                    num_of_deposits = int(input("Введите количество вкладов (1–100): "))
                    if 1 <= num_of_deposits <= 100:
                        break
                    else:
                        print("Количество вкладов должно быть от 1 до 100.")
                except ValueError:
                    print("Введите корректное число.")

           
            while True:
                try:
                    deposit_size = float(input("Введите размер одного вклада (1000–1,000,000): "))
                    if 1000 <= deposit_size <= 1_000_000:
                        break
                    else:
                        print("Размер вклада должен быть от 1000 до 1,000,000.")
                except ValueError:
                    print("Введите корректное число.")

          
            while True:
                print("Выберите тип вклада:")
                print("1. Стандартный вклад")
                print("2. Вклад с бонусом")
                deposit_choice = input("Введите номер типа вклада: ")
                if deposit_choice == "1":
                    deposit_type = StandardDeposit()
                    break
                elif deposit_choice == "2":
                    deposit_type = BonusDeposit()
                    break
                else:
                    print("Неверный выбор! Попробуйте снова.")

           
            bank.add_new_client(name, num_of_deposits, deposit_size, deposit_type)

        elif choice == "2":
            bank.bank_info()

        elif choice == "3":
            total_deposits = bank.calculate_total_deposits()
            print(f"Общая сумма вкладов в банке: {total_deposits}")

        elif choice == "4":
            print("Выход из программы...")
            break

        else:
            print("Неверный выбор, пожалуйста, попробуйте снова.")



run_bank_system()
