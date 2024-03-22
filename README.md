# Reto número 4 repo POO

 ### **Fecha:** 06-03-2024

**1.** Class exercise:

* Mirar archivo ejercicio_en_clase_2.py

```python
import math

class Point:
    definition = "Abstract geometric entity that represents a location in space."

    def __init__(self, x:float, y:float):
        self.x = x
        self.y = y

    def compute_distance(self, start_point, end_point):
        distance = math.sqrt((start_point.x - end_point.x) ** 2 + (start_point.y - end_point.y) ** 2)
        return distance
    
class Line:
    def __init__(self, start_point:Point, end_point:Point):
        self.start_point = start_point
        self.end_point = end_point
        self.length = start_point.compute_distance(start_point, end_point)
        
        delta_x = self.end_point.x - self.start_point.x
        delta_y = self.end_point.y - self.start_point.y
        
        if delta_x != 0: 
            self.slope = delta_y/delta_x

class Shape:
    def __init__(self,  is_regular:bool, vertices:list, edges:list, inner_angles:list):
        self.is_regular = is_regular 
        self.vertices =  vertices
        self.edges=edges
        self.inner_angles=inner_angles

    def compute_edges(self):
        self.edges = []
        for i in range(len(self.vertices)):
            start_point = self.vertices[i]
            end_point = self.vertices[(i+1)%len(self.vertices)] # Para que en la lista se una el ultimo punto con el primero
            line = Line(start_point, end_point)
            self.edges.append(line.length)
        return self.edges
    
    def compute_inner_angles(self):
        pass

    def compute_perimeter(self):
        pass

    def compute_area(self):
        pass

class Triangle(Shape):                
    def __init__(self,  is_regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(is_regular, vertices, edges, inner_angles)
    
    # Ya esta heredando compute_edges y es el mismo, no es necesario volverlo a copiar
    
    def compute_inner_angles(self):
        if len(self.edges)==3:
            a, b, c = self.edges  # Lados del triángulo
            angulo_a = math.acos((b**2 + c**2 - a**2) / (2 * b * c))
            angulo_b = math.acos((a**2 + c**2 - b**2) / (2 * a * c))
            angulo_c = math.pi - angulo_a - angulo_b  # El ángulo restante
            self.inner_angles = [math.degrees(angulo_a), math.degrees(angulo_b), math.degrees(angulo_c)]
        return self.inner_angles

    def compute_perimeter(self):
        if len(self.edges)==3:
            self.perimeter = sum(self.edges)
        return self.perimeter

    def compute_area(self):
        pass
        
class Rectangle(Shape):                  
    def __init__(self,  is_regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(is_regular, vertices, edges, inner_angles)

    # Ya esta heredando compute_edges y es el mismo, no es necesario volverlo a copiar
    
    def compute_inner_angles(self):
        if len(self.edges)==4:
            self.inner_angles = [90, 90, 90, 90]
        return self.inner_angles
    
    def compute_perimeter(self):
        if len(self.edges)==4:
            self.perimeter = sum(self.edges)
        return self.perimeter

    def compute_area(self, base, altura):
        if len(self.edges)==4:
            base = max(self.edges)
            altura = min(self.edges)
            self.area = (base*altura)
        return self.area

class Square(Rectangle):             
    def __init__(self,  is_regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(is_regular, vertices, edges, inner_angles)
    
    # Hereda todas las funciones y le sirve esa misma
    
class Isosceles(Triangle):              # Hereda de Triangle
    def __init__(self,  is_regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(is_regular, vertices, edges, inner_angles)
    
    # La función def compute_inner_angles se hereda y funciona
    # La función def compute_perimeter se hereda y funciona
    
    def compute_area(self):
        if len(self.edges)==3:
            altura = math.sqrt((self.edges[0])**2 - ((self.edges[1])**2/4))
            self.area = 0.5 * self.edges[1] * altura
        return self.area

class Equilateral(Triangle):              # Hereda de Triangle
    def __init__(self,  is_regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(is_regular, vertices, edges, inner_angles)
    
    # La función def compute_inner_angles se hereda y funciona
    # La función def compute_perimeter se hereda y funciona
    
    def compute_area(self):
        if len(self.edges)==3:
            area = (math.sqrt(3) / 4)*(self.edges[0]**2)
        return area

class Scalene(Triangle):
    def __init__(self,  is_regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(is_regular, vertices, edges, inner_angles)
    
    # La función def compute_inner_angles se hereda y funciona
    # La función def compute_perimeter se hereda y funciona
    
    def compute_area(self):
        if len(self.edges)==3:
            semiperimetro = sum(self.edges)/2
            area = math.sqrt(semiperimetro*(semiperimetro-self.edges[0])*(semiperimetro-self.edges[1])*(semiperimetro-self.edges[2]))
        return area

class Rectangle(Triangle):
    def __init__(self,  is_regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(is_regular, vertices, edges, inner_angles)
    
    # La función def compute_inner_angles se hereda y funciona
    # La función def compute_perimeter se hereda y funciona
    
    def compute_area(self):
        if len(self.edges)==3:
            base = self.edges[0]
            altura = self.edges[1]
            self.area=0.5*base*altura
        return self.area

if __name__ == "__main__": # Función main
    
```
**2.** The restaurant revisted

* Mirar archivo restaurant_mejorado.py

```python
# Definición de la clase MenuItem para representar elementos del menú con nombre y precio
class MenuItem:
    def __init__(self, name, price):
        self._name = name  # Nombre del elemento del menú (protegido)
        self._price = price  # Precio del elemento del menú

    # Método para obtener el nombre del elemento del menú
    def get_name(self):
        return self._name

    # Método para establecer el nombre del elemento del menú
    def set_name(self, name):
        self._name = name

    # Método para obtener el precio del elemento del menú
    def get_price(self):
        return self._price

    # Método para establecer el precio del elemento del menú
    def set_price(self, price):
        self._price = price

# Definición de la clase MedioPago para representar diferentes métodos de pago
class MedioPago:
    def __init__(self):
        pass

    # Método abstracto para realizar un pago con un monto específico
    def pagar(self, monto):
        pass

# Definición de la clase Tarjeta que hereda de MedioPago y representa el pago con tarjeta
class Tarjeta(MedioPago):
    def __init__(self, numero, cvv):
        super().__init__()
        self.numero = numero  # Número de tarjeta
        self.cvv = cvv  # Código CVV de la tarjeta

    # Método para realizar un pago con la tarjeta
    def pagar(self, monto):
        print(f"Pagando {monto} con tarjeta {self.numero[-4:]}")  # Se muestra el monto y los últimos 4 dígitos de la tarjeta

# Definición de la clase Efectivo que hereda de MedioPago y representa el pago en efectivo
class Efectivo(MedioPago):
    def __init__(self, monto_entregado):
        super().__init__()
        self.monto_entregado = monto_entregado  # Monto entregado en efectivo

    # Método para realizar un pago en efectivo
    def pagar(self, monto):
        if self.monto_entregado >= monto:  # Si el monto entregado es suficiente para cubrir el pago
            print(f"Pago realizado en efectivo. Cambio: {self.monto_entregado - monto}")  # Se muestra el cambio
        else:  # Si el monto entregado es insuficiente
            print(f"Fondos insuficientes. Faltan {monto - self.monto_entregado} para completar el pago.")  # Se muestra la cantidad faltante

# Definición de las clases para diferentes elementos del menú (Appetizer, MainCourse, SideDish, Dessert, Beverage),
# que heredan de MenuItem y representan elementos específicos del menú con sus precios
# y métodos para seleccionar el elemento y obtener su precio

class Appetizer(MenuItem):
    def __init__(self, name):
        super().__init__(name, price=0)

    def item_selected(self):
        name = self.get_name()
        if name == "bruschetta":
            return 7.99
        elif name == "spinach and artichoke dip":
            return 8.49
        elif name == "caprese salad":
            return 9.99
        elif name == "stuffed mushrooms":
            return 10.49
        elif name == "shrimp cocktail":
            return 12.99
        else:
            return 0

class MainCourse(MenuItem):
    def __init__(self, name):
        super().__init__(name, price=0)

    def item_selected(self):
        name = self.get_name()
        if name == "grilled salmon":
            return 16.99
        elif name == "chicken parmesan":
            return 14.99
        elif name == "beef tenderloin steak":
            return 22.99
        elif name == "vegetable stir fry":
            return 12.99
        elif name == "pasta with grilled chicken":
            return 15.49
        else:
            return 0

class SideDish(MenuItem):
    def __init__(self, name):
        super().__init__(name, price=0)

    def item_selected(self):
        name = self.get_name()
        if name == "french fries":
            return 3.99
        elif name == "mashed potatoes":
            return 4.49
        elif name == "steamed vegetables":
            return 5.29
        elif name == "garlic bread":
            return 3.99
        elif name == "onion rings":
            return 4.99
        else:
            return 0

class Dessert(MenuItem):
    def __init__(self, name):
        super().__init__(name, price=0)

    def item_selected(self):
        name = self.get_name()
        if name == "chocolate cake":
            return 6.99
        elif name == "cheesecake":
            return 5.49
        elif name == "tiramisu":
            return 7.99
        elif name == "apple pie":
            return 4.99
        elif name == "ice cream":
            return 4.79
        else:
            return 0

class Beverage(MenuItem):
    def __init__(self, name):
        super().__init__(name, price=0)

    def item_selected(self):
        name = self.get_name()
        if name == "soda":
            return 1.99
        elif name == "juice":
            return 2.49
        elif name == "coffee":
            return 2.99
        elif name == "iced tea":
            return 2.29
        elif name == "smoothie":
            return 4.99
        else:
            return 0

# Definición de la clase Order para representar una orden con una lista de elementos, precios, y método de pago
class Order:
    def __init__(self):
        self.items = []  # Lista de elementos de la orden
        self.prices = []  # Lista de precios de los elementos de la orden
        self.medio_pago = None  # Método de pago para la orden

    # Método para mostrar el menú disponible
    def show_menu(self):
        # Se imprime el menú con los elementos y precios disponibles
        print("Alejas Restaurant")

        print("\nAppetizer:")
        print("Bruschetta..............................7.99")
        print("Spinach and artichoke dip...............8.49")
        print("Caprese salad...........................9.99")
        print("Stuffed mushrooms......................10.49")
        print("Shrimp cocktail........................12.99")

        print("\nMain Course:")
        print("Grilled salmon.........................16.99")
        print("Chicken parmesan.......................14.99")
        print("Beef tenderloin steak..................22.99")
        print("Vegetable stir fry.....................12.99")
        print("Pasta with grilled chicken.............15.49")

        print("\nSide Dish")
        print("French fries............................3.99")
        print("Mashed potatoes.........................4.49")
        print("Steamed vegetables......................5.29")
        print("Garlic bread............................3.99")
        print("Onion rings.............................4.99")

        print("\nDessert")
        print("Chocolate cake..........................6.99")
        print("Cheesecake..............................5.49")
        print("Tiramisu................................7.99")
        print("Apple pie...............................4.99")
        print("Ice cream...............................4.79")

        print("\nBeverage")
        print("Soda...................................1.99")
        print("Juice..................................2.49")
        print("Coffee.................................2.99")
        print("Iced tea...............................2.29")
        print("Smoothie...............................4.99")

        print("\n")

     # Método para tomar la orden de los clientes, agregar elementos a la orden y calcular los precios
    def order_items(self):
        # Se solicita la cantidad y los elementos de cada tipo del menú al cliente, se crean los objetos correspondientes
        # y se agregan a la orden junto con sus precios calculados
        items = self.items
        prices = self.prices

        quantity_appetizer = int(input("Insert the quantity of appetizer: "))
        for _ in range(quantity_appetizer):
            name_appetizer = input("Insert your Appetizer: ").lower()
            appetizer = Appetizer(name_appetizer)
            items.append(appetizer)
            price = appetizer.item_selected()
            prices.append(price)

        self.quantity_maincourse = int(input("Insert the quantity of main course: "))
        for _ in range(self.quantity_maincourse):
            name_maincourse = input("Insert your MainCourse: ").lower()
            maincourse = MainCourse(name_maincourse)
            items.append(maincourse)
            price = maincourse.item_selected()
            prices.append(price)

        quantity_sidedish = int(input("Insert the quantity of side dish: "))
        for _ in range(quantity_sidedish):
            name_sidedish = input("Insert your SideDish: ").lower()
            sidedish = SideDish(name_sidedish)
            items.append(sidedish)
            price = sidedish.item_selected()
            prices.append(price)

        self.quantity_dessert = int(input("Insert the quantity of dessert: "))
        for _ in range(self.quantity_dessert):
            name_dessert = input("Insert your Dessert: ").lower()
            dessert = Dessert(name_dessert)
            items.append(dessert)
            price = dessert.item_selected()
            prices.append(price)

        quantity_beverage = int(input("Insert the quantity of beverage: "))
        for _ in range(quantity_beverage):
            name_beverage = input("Insert your Beverage: ").lower()
            beverage = Beverage(name_beverage)
            items.append(beverage)
            price = beverage.item_selected()
            prices.append(price)

    # Método para establecer el método de pago para la orden
    def set_medio_pago(self, medio_pago):
        self.medio_pago = medio_pago  # Se asigna el método de pago proporcionado a la orden

    # Método para calcular el total de la orden
    def calculate_total_bill(self):
        return sum(self.prices)  # Se suma el total de los precios de los elementos en la orden

    # Método para aplicar descuentos a la orden según ciertas condiciones
    def discounts(self):
        # Se calcula el total de la orden y se aplica un descuento del 5% si se cumplen ciertas condiciones
        total_bill = self.calculate_total_bill()
        if self.quantity_maincourse >= 2 and self.quantity_dessert>=1:
            return total_bill - ((total_bill * 5) / 100)
        else:
            return total_bill 

     # Método para imprimir la factura de la orden con los elementos, precios, total y método de pago
    def print_bill(self):
        # Se imprime la factura con los elementos de la orden, precios individuales, total, descuentos (si aplican)
        # y se muestra el método de pago utilizado en la orden
        items = self.items
        total_bill = self.calculate_total_bill()
        discounts = self.discounts()

        print("Your bill:\n")
        for item in items:
            name = item.get_name()
            price = 0.0
            if name == "bruschetta":
                price = 7.99
            elif name == "spinach and artichoke dip":
                price = 8.49
            elif name == "caprese salad":
                price = 9.99
            elif name == "stuffed mushrooms":
                price = 10.49
            elif name == "shrimp cocktail":
                price = 12.99
            elif name == "grilled salmon":
                price = 16.99
            elif name == "chicken parmesan":
                price = 14.99
            elif name == "beef tenderloin steak":
                price = 22.99
            elif name == "vegetable stir fry":
                price = 12.99
            elif name == "pasta with grilled chicken":
                price = 15.49
            elif name == "french fries":
                price = 3.99
            elif name == "mashed potatoes":
                price = 4.49
            elif name == "steamed vegetables":
                price = 5.29
            elif name == "garlic bread":
                price = 3.99
            elif name == "onion rings":
                price = 4.99
            elif name == "chocolate cake":
                price = 6.99
            elif name == "cheesecake":
                price = 5.49
            elif name == "tiramisu":
                price = 7.99
            elif name == "apple pie":
                price = 4.99
            elif name == "ice cream":
                price = 4.79
            elif name == "soda":
                price = 1.99
            elif name == "juice":
                price = 2.49
            elif name == "coffee":
                price = 2.99
            elif name == "iced tea":
                price = 2.29
            elif name == "smoothie":
                price = 4.99
            print("{:<27}{}".format(name.capitalize(), "{:.2f}".format(price)))  # El format es para que se formatee cada vez

        print("\n")
        print("Your order costs: " + str(total_bill))
        if discounts != total_bill:
            print("\n")
            print("Congrats! You obtain a discount of 5% on your bill")
            print("Your total cost is: " + str(discounts))

        if self.medio_pago is not None:
            print("\n")
            self.medio_pago.pagar(total_bill)

if __name__ == "__main__": # Función main
    my_order = Order()
    my_order.show_menu()
    my_order.order_items()
    print("\n")

    while True:
        metodo_pago = input("Select payment method (1 for Cash, 2 for Card): ")
        if metodo_pago == '1':
            monto_entregado = float(input("Enter the amount of cash provided: "))
            my_order.set_medio_pago(Efectivo(monto_entregado))
            break
        elif metodo_pago == '2':
            numero_tarjeta = input("Enter the card number: ")
            cvv_tarjeta = input("Enter the CVV code: ")
            my_order.set_medio_pago(Tarjeta(numero_tarjeta, cvv_tarjeta))
            break
        else:
            print("Invalid option. Please try again.")

    print("\n")
    my_order.print_bill()
```
