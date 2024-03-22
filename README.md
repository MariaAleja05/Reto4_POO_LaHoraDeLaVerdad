# Reto número 4 repo POO

 ### **Fecha:** 06-03-2024

**1.** Class exercise:

* Mirar archivo Figuritas_y_sus_hijos.py

```python
import math

class Point:
    definition = "Abstract geometric entity that represents a location in space."

    def __init__(self, x:float, y:float):
        self._x = x  # Coordenada x del punto
        self._y = y  # Coordenada y del punto
    
    def get_x(self):  # Getter x
        return self._x

    def set_x(self, x: float):  # Setter x
        self._x = x

    def get_y(self):  # Getter y
        return self._y

    def set_y(self, y: float):  # Setter y
        self._y = y

    def compute_distance(self, start_point, end_point):
        # Calcula la distancia entre dos puntos utilizando la fórmula de distancia euclidiana
        distance = math.sqrt((start_point.get_x() - end_point.get_x()) ** 2 + (start_point.get_y() - end_point.get_y()) ** 2)
        return distance
    
class Line:
    def __init__(self, start_point:Point, end_point:Point):
        self.start_point = start_point  # Punto inicial de la línea
        self.end_point = end_point  # Punto final de la línea
        self.length = start_point.compute_distance(start_point, end_point)  # Longitud de la línea
        
        delta_x = self.end_point.get_x() - self.start_point.get_x()
        delta_y = self.end_point.get_y() - self.start_point.get_y()
        
        if delta_x != 0: 
            self.slope = delta_y / delta_x  # Pendiente de la línea si no es vertical

class Shape:
    def __init__(self, is_regular:bool, vertices:list, edges:list, inner_angles:list):
        self.is_regular = is_regular  # Indica si la forma es regular
        self.vertices = vertices  # Lista de puntos que forman la forma
        self.edges = edges  # Lista de longitudes de los lados
        self.inner_angles = inner_angles  # Lista de ángulos interiores

    def compute_edges(self):
        # Calcula las longitudes de los lados de la forma
        self.edges = []
        for i in range(len(self.vertices)):
            start_point = self.vertices[i]
            end_point = self.vertices[(i+1) % len(self.vertices)]  # Conecta el último punto con el primero
            line = Line(start_point, end_point)
            self.edges.append(line.length)
        return self.edges
    
    def compute_inner_angles(self):
        pass  # Método para calcular los ángulos interiores de la forma (a implementar en las subclases)

    def compute_perimeter(self):
        pass  # Método para calcular el perímetro de la forma (a implementar en las subclases)

    def compute_area(self):
        pass  # Método para calcular el área de la forma (a implementar en las subclases)

class Triangle(Shape):                
    def __init__(self, is_regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(is_regular, vertices, edges, inner_angles)
    
    def compute_inner_angles(self):
        if len(self.edges) == 3:
            a, b, c = self.edges  # Lados del triángulo
            angulo_a = math.acos((b**2 + c**2 - a**2) / (2 * b * c))
            angulo_b = math.acos((a**2 + c**2 - b**2) / (2 * a * c))
            angulo_c = math.pi - angulo_a - angulo_b  # Ángulo restante
            self.inner_angles = [math.degrees(angulo_a), math.degrees(angulo_b), math.degrees(angulo_c)]
        return self.inner_angles

    def compute_perimeter(self):
        if len(self.edges) == 3:
            self.perimeter = sum(self.edges)  # Suma de los lados del triángulo
        return self.perimeter

    def compute_area(self):
        pass  # Método para calcular el área del triángulo (a implementar)

class Rectangle(Shape):                  
    def __init__(self, is_regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(is_regular, vertices, edges, inner_angles)

    def compute_inner_angles(self):
        if len(self.edges) == 4:
            self.inner_angles = [90, 90, 90, 90]  # Ángulos interiores de un rectángulo
        return self.inner_angles
    
    def compute_perimeter(self):
        if len(self.edges) == 4:
            self.perimeter = sum(self.edges)  # Suma de los lados del rectángulo
        return self.perimeter

    def compute_area(self):
        if len(self.edges) == 4:
            base = max(self.edges)  # Lado más largo (base)
            altura = min(self.edges)  # Lado más corto (altura)
            self.area = base * altura  # Área del rectángulo
        return self.area

class Square(Rectangle):             
    def __init__(self, is_regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(is_regular, vertices, edges, inner_angles)
    
    # Hereda todas las funciones de Rectangle
    
class Isosceles(Triangle):              # Hereda de Triangle
    def __init__(self, is_regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(is_regular, vertices, edges, inner_angles)
    
    # La función compute_inner_angles se hereda y funciona
    # La función compute_perimeter se hereda y funciona
    
    def compute_area(self):
        if len(self.edges) == 3:
            # Calcula el área de un triángulo isósceles utilizando la fórmula de altura
            altura = math.sqrt((self.edges[0])**2 - ((self.edges[1])**2/4))
            self.area = 0.5 * self.edges[1] * altura
        return self.area

class Equilateral(Triangle):              # Hereda de Triangle
    def __init__(self, is_regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(is_regular, vertices, edges, inner_angles)
    
    # La función compute_inner_angles se hereda y funciona
    # La función compute_perimeter se hereda y funciona
    
    def compute_area(self):
        if len(self.edges) == 3:
            # Calcula el área de un triángulo equilátero utilizando la fórmula específica
            area = (math.sqrt(3) / 4) * (self.edges[0] ** 2)
        return area

class Scalene(Triangle):
    def __init__(self, is_regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(is_regular, vertices, edges, inner_angles)
    
    # La función compute_inner_angles se hereda y funciona
    # La función compute_perimeter se hereda y funciona
    
    def compute_area(self):
        if len(self.edges) == 3:
            semiperimeter = sum(self.edges) / 2  # Semiperímetro del triángulo
            # Calcula el área de un triángulo escaleno utilizando la fórmula de Herón
            area = math.sqrt(semiperimeter * (semiperimeter - self.edges[0]) * 
                             (semiperimeter - self.edges[1]) * (semiperimeter - self.edges[2]))
        return area

class TriRectangle(Triangle):
    def __init__(self, is_regular:bool, vertices:list, edges:list, inner_angles:list):
        super().__init__(is_regular, vertices, edges, inner_angles)
    
    # La función compute_inner_angles se hereda y funciona
    # La función compute_perimeter se hereda y funciona
    
    def compute_area(self):
        if len(self.edges) == 3:
            base = self.edges[0]  # El lado que se toma como base del rectángulo
            altura = self.edges[1]  # La altura del rectángulo
            self.area = 0.5 * base * altura  # Fórmula del área de un rectángulo
        return self.area

# Ahora el código principal que utiliza estas clases para calcular las propiedades de las formas geométricas
if __name__ == "__main__":
    def t_isoceles():       # Para probar triangulo isoceles
        A = Point(0,0)
        B = Point(4,0)
        C = Point(2,3)
        isosceles=Isosceles(True, [A,B,C], [], [])
        edges_isoceles = isosceles.compute_edges()
        inner_angles_isoceles = isosceles.compute_inner_angles()
        perimeter_isosceles = isosceles.compute_perimeter()
        area_isosceles = isosceles.compute_area()
        print("\nTriangulo Isoceles:")
        print("Edges: ", edges_isoceles)
        print("Inner angles: ", inner_angles_isoceles)
        print("Perimeter: ", perimeter_isosceles)
        print("Area: ", area_isosceles)

    def t_equilateral():    # Para probar triangulo equilatero
        A = Point(0,0)
        B = Point(3,0)
        C = Point(1.5,2.598)
        equilateral=Equilateral(True, [A,B,C], [], [])
        edges_equilateral = equilateral.compute_edges()
        inner_angles_equilateral = equilateral.compute_inner_angles()
        perimeter_equilateral = equilateral.compute_perimeter()
        area_equilateral = equilateral.compute_area()
        print("\nTriangulo Equilateral:")
        print("Edges: ", edges_equilateral)
        print("Inner angles: ", inner_angles_equilateral)
        print("Perimeter: ", perimeter_equilateral)
        print("Area: ", area_equilateral)

    def t_scalene():        # Para probar triangulo escaleno
        A = Point(-1,2)
        B = Point(3,0)
        C = Point(0,-3)
        scalene=Scalene(True, [A,B,C], [], [])
        edges_scalene = scalene.compute_edges()
        inner_angles_scalene = scalene.compute_inner_angles()
        perimeter_scalene = scalene.compute_perimeter()
        area_scalene = scalene.compute_area()
        print("\nTriangulo Scalene:")
        print("Edges: ", edges_scalene)
        print("Inner angles: ", inner_angles_scalene)
        print("Perimeter: ", perimeter_scalene)
        print("Area: ", area_scalene)

    def t_trirectangle():   # Para probar triangulo rectangulo
        A = Point(-1,2)
        B = Point(3,0)
        C = Point(0,-3)
        trirectangle=TriRectangle(True, [A,B,C], [], [])
        edges_rectangle = trirectangle.compute_edges()
        inner_angles_trirectangle = trirectangle.compute_inner_angles()
        perimeter_trirectangle = trirectangle.compute_perimeter()
        area_trirectangle = trirectangle.compute_area()
        print("\nTriangulo Rectangle:")
        print("Edges: ", edges_rectangle)
        print("Inner angles: ", inner_angles_trirectangle)
        print("Perimeter: ", perimeter_trirectangle)
        print("Area: ", area_trirectangle)

    def r_square():         # Para probar cuadrado
        A = Point(0,0)
        B = Point(0,4)
        C = Point(4,4)
        D = Point(4,0)
        square=Square(True, [A,B,C,D], [], [])
        edges_square = square.compute_edges()
        inner_angles_square = square.compute_inner_angles()
        perimeter_square = square.compute_perimeter()
        area_square = square.compute_area()
        print("\nRectangle Square:")
        print("Edges: ", edges_square)
        print("Inner angles: ", inner_angles_square)
        print("Perimeter: ", perimeter_square)
        print("Area: ", area_square)


    # Para mostrar resultados
    t_isoceles()
    t_equilateral()
    t_scalene()
    t_trirectangle()
    r_square()
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
