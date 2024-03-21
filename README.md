# Reto número 4 repo POO

 ### **Fecha:** 06-03-2024

**1.** Class exercise:

* Mirar archivo ejercicio_en_clase_2.py

```python
import numpy as np
import math

class Point:
    def __init__(self, x:float, y:float):
        self.x = x
        self.y = y

    def compute_distance(self, end_point):
        distance = math.sqrt((self.x - end_point.x) ** 2 + (self.y - end_point.y) ** 2)
        return distance
    
class Line:
    def __init__(self, lenght:float, slope:float, start_point:Point, end_point:Point):
        self.start_point = start_point
        self.end_point = end_point
        self.lenght = start_point.compute_distance(end_point)
        
        delta_x = self.end_point.x - self.start_point.x
        delta_y = self.end_point.y - self.start_point.y
        
        if delta_x != 0: 
            self.slope = delta_y/delta_x

class Shape:
    def __init__(self,  is_regular:bool, vertices:list, edges:list, inner_angles:list):
        self.is_regular = is_regular 
        self.vertices =  vertices
        self.edges =  edges
        self.inner_angles =  inner_angles
        self.point = Point()
        self.line = Line()

    def compute_area(self):
        pass

    def compute_perimeter(self):
        pass

    def compute_inner_angles(self):
        pass

class Triangle(Shape):                
    def __init__(self,  is_regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(is_regular, vertices, edges, inner_angles)

    def compute_area(self):
        pass

    def compute_perimeter(self):
        pass

    def compute_inner_angles(self): #####OJOOOOO
        if len(edges) == 3:
            #I use the cosine teorem to calcole each angle
            Angle_A=math.degrees(np.arccos((((edges[1])**2)+(edges[2]**2)-(edges[0]**2))/(2*edges[1]*edges[2])))
            Angle_B=math.degrees(np.arccos((((edges[0])**2)+(edges[2]**2)-(edges[1]**2))/(2*edges[0]*edges[2])))
            Angle_C= math.degrees(np.arccos((((edges[0])**2)+(edges[1]**2)-(edges[2]**2))/(2*edges[0]*edges[1])))
        inner_angles=[Angle_A,Angle_B,Angle_C]
        return inner_angles

class Rectangle(Shape):                  
    def __init__(self,  is_regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(is_regular, vertices, edges, inner_angles)

    def compute_area(self, base, altura):
        self.area = (base*altura)
        return self.area

    def compute_perimeter(self, base, altura):
        self.perimeter = (base*2)+(altura*2)

    def compute_inner_angles(self):
        self.inner_angles=[90,90,90,90]
        for i in len(self.inner_angles):
            total_inner_angles=total_inner_angles+self.inner_angles[i]
        return total_inner_angles

class Square(Rectangle):             
    def __init__(self,  is_regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(is_regular, vertices, edges, inner_angles)
    
    def compute_area(self, base, altura):
        self.area = (base*altura)
        return self.area

    def compute_perimeter(self, base, altura):
        self.perimeter = (base*2)+(altura*2)

    def compute_inner_angles(self):
        self.inner_angles=[90,90,90,90]
        for i in len(self.inner_angles):
            total_inner_angles=total_inner_angles+self.inner_angles[i]
        return total_inner_angles
    
class Isosceles(Triangle):              # Hereda de Triangle
    def __init__(self,  is_regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(is_regular, vertices, edges, inner_angles)
    
    def compute_area(self, base, altura):
        self.area = (base*altura)/2
        return self.area

    def compute_perimeter(self, longitud_lado1, longitud_lado2, longitud_lado3):
        self.perimeter = longitud_lado1+longitud_lado2+longitud_lado3
        return

    def compute_inner_angles(self):
        self.inner_angles=[angle]
        for i in len(self.inner_angles):
            total_inner_angles=total_inner_angles+self.inner_angles[i]
        return total_inner_angles

class Equilateral(Triangle):
    def __init__(self,  is_regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(is_regular, vertices, edges, inner_angles)
    def compute_area(self):
class Scalene(Triangle):
    def __init__(self,  is_regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(is_regular, vertices, edges, inner_angles)
    def compute_area(self):
class TriRectangle(Triangle):
    def __init__(self,  is_regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(is_regular, vertices, edges, inner_angles)
    def compute_area(self):

# Crear puntos
p1 = Point(0, 0)
p2 = Point(3, 4)
p3 = Point(6, 0)

# Crear líneas
line1 = Line(p1, p2)
line2 = Line(p2, p3)
line3 = Line(p3, p1)

# Crear triángulo
triangulo = Triangle(True, [p1, p2, p3], [line1, line2, line3], [90, 45, 45])

# Calcular área del triángulo
base = line1.length
altura = line2.length
area_tri = triangulo.compute_area(base, altura)

# Crear rectángulo
rectangulo = Rectangle(True, [p1, p2, p3], [line1, line2, line3], [90, 90, 90, 90])

# Calcular área del rectángulo
area_rect = rectangulo.compute_area(base, altura)

# Calcular perímetro del rectángulo
perim_rect = rectangulo.compute_perimeter(base, altura)

# Calcular suma de ángulos internos del triángulo
suma_angulos_tri = triangulo.compute_inner_angles()

# Calcular suma de ángulos internos del rectángulo
suma_angulos_rect = rectangulo.compute_inner_angles()

# Imprimir resultados
print("Área del triángulo:", area_tri)
print("Área del rectángulo:", area_rect)
print("Perímetro del rectángulo:", perim_rect)
print("Suma de ángulos internos del triángulo:", suma_angulos_tri)
print("Suma de ángulos internos del rectángulo:", suma_angulos_rect)
```
**2.** The restaurant revisted

* Mirar archivo restaurant_mejorado.py

```python
class MenuItem:
    def __init__(self, name, price):
        self._name = name  # Protegido
        self._price = price

    def get_name(self):
        return self._name

    def set_name(self, name):
        self._name = name

    def get_price(self):
        return self._price

    def set_price(self, price):
        self._price = price

class MedioPago:
    def __init__(self):
        pass

    def pagar(self, monto):
        pass

class Tarjeta(MedioPago):
    def __init__(self, numero, cvv):
        super().__init__()
        self.numero = numero
        self.cvv = cvv

    def pagar(self, monto):
        print(f"Pagando {monto} con tarjeta {self.numero[-4:]}")

class Efectivo(MedioPago):
    def __init__(self, monto_entregado):
        super().__init__()
        self.monto_entregado = monto_entregado

    def pagar(self, monto):
        if self.monto_entregado >= monto:
            print(f"Pago realizado en efectivo. Cambio: {self.monto_entregado - monto}")
        else:
            print(f"Fondos insuficientes. Faltan {monto - self.monto_entregado} para completar el pago.")

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

class Order:
    def __init__(self):
        self.items = []
        self.prices = []
        self.medio_pago = None

    def show_menu(self):  # Se muestra el menu
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

    def order_items(self):
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

    def set_medio_pago(self, medio_pago):
        self.medio_pago = medio_pago

    def calculate_total_bill(self):
        return sum(self.prices)

    def discounts(self):
        total_bill = self.calculate_total_bill()
        if self.quantity_maincourse >= 2 and self.quantity_dessert>=1:
            return total_bill - ((total_bill * 5) / 100)
        else:
            return total_bill 

    def print_bill(self):
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
