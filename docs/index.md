# Shop and Inventory System with `Screens` for `Ren'py`
## Explanation of the functions incorporated in this system
In order to use this multi-system that includes or incorporates through functions linked to each other, the following systems:

- Currency 
- Store
- Cart or shopping list
- Inventory of common objects
- Inventory of consumable items
- Effects of consumable items in points system

We must know how these work and investigate some small details to be able to execute them correctly when using them in our `Ren'py` project.

### NOTE
Before defining or creating this set of systems, it must be located within an `Init Python` block in any of the `RPY` files in the `game/` folder for it to function correctly, as shown in the following `Markdown' `:
```py
Init Python:
   class Item:
      def __init__(self, name, cost, tag=None, effect=None):
         self.name = name 
         self.cost = cost 
         self.tag = tag 
         self.effect = effect 
         self.num = 0 
         self.amount = 0 
         self.box = 0

   class System:
      def __init__(self):
         self.money = 0
         self.total_cost = 0
         self.total_cost_num = 0
         self.items = []
         self.items_car = []
         self.items_num = []
         self.items_num_car = []
         
      def earn(self, amount):
         self.money += amount

      def buy(self):
         if self.money >=  self.total_cost: 
            self.money -=  self.total_cost
            self.total_cost = 0
            self.items.extend(self.items_car)
            self.items_car.clear()
            renpy.notify("Successful purchase")
         else:
            renpy.notify("Insufficient Balance")

      def add_item(self, item):
         if self.money >= self.total_cost + item.cost
            self.total_cost += item.cost 
            self.items_car.append(item) 
         else:
            renpy.notify("Insufficient Balance") 

      def remove_item(self, item):
         if item in self.items_car:
            self.total_cost -= item.cost
            self.items_car.remove(item)

      def has_item_car(self, item):
         if item in self.items_car: 
            return True 
         else:
            return False 

      def has_item(self, item):
         if item in self.items: 
            return True 
         else:
            return False

      def buy_items(self):
         if self.money >=  self.total_cost_num:
            self.money -=  self.total_cost_num 
            self.total_cost_num = 0 
            self.items_num.extend(self.items_num_car) 
            self.items_num_car.clear()
            self.reset_items()
            renpy.notify("Successful purchase")
         else:
            renpy.notify("Insufficient Balance")

      def reset_items(self):
         if energize in self.items_num: 
            energize.num += energize.box 
            energize.box = 0 
         else: 
            pass

      def add_items(self, item):
         if self.money >= self.total_cost_num + item.cost: 
            if item in self.items_num_car: 
               if item.box < 99:
                  self.total_cost += item.cost
                  item.box += 1
               else: 
                  renpy.notify("Maximum Achieved")
            elif not item in self.items_num_car:
               self.total_cost_num += item.cost 
               item.box += 1
               self.items_num_car.append(item)
         else:
            renpy.notify("Insufficient Balance")

      def remove_items(self, item):
         if item in self.items_num_car: 
            if item.box > 1:
               self.total_cost_num -= item.cost 
               item.box -= 1
            else:
               self.total_cost -= item.cost
               item.box = 0
               self.items_num_car.remove(item)

      def delete_items(self, item):
         if item in self.items_num_car: 
            self.total_cost_num -= item.cost * item.box 
            item.box = 0 
            self.items_num_car.remove(item)

      def not_use(self, item):
         if item in self.items_num:
            if item.num - item.amount + 1 <= item.num: 
               item.amount -= 1 
            elif item.num - item.amount == item.num: 
               renpy.notify("Maximum Achieved") 

      def use(self, item):
         if item in self.items_num: 
            if item.tag == "energy": 
               if store.energy + item.effect*item.amount < 100 and item.num - item.amount -1 > 0:
                  item.amount += 1 
               else:
                  renpy.notify(f"cannot be used {item.name}") 

      def consume(self, item):
         if item in self.items_num: 
            if item.num - item.amount >= 0:
               self.effect_use(item) 
               item.num -= item.amount 
               item.amount = 0 
               if item.num == 0: 
                  self.items_num.remove(item) 
               renpy.notify(f"It was used {item.name}")

      def effect_use(self, item):
         if item.tag == "energy": 
            store.energy += item.effect*item.amount
            if store.energy > 100: 
               store.energy = 100

      def has_items_car(self, item):
         if item in self.items_num_car: 
            return True 
         else:
            return False 

      def has_items(self, item):
         if item in self.items_num: 
            return True
         else:
            return False
```

### How can I use or call class functions?
#### In `Label` with `menu`:
The functions in `Label` and `menu` blocks are called by placing `$` in front of the class defined as an object divided by a `.` followed by the name of the function to be used and a `()` (in this case function requires a parameter to be defined, these parameters must be located within `()` and if there are more than one, separated by `,` from each other).

Examples:
```py
Label example:
   $ system = System()

   "I currently have %[system.money]d as money"
   "What's that glowing on the ground?"

   $ system.earn(50) #<-- Call the `earn` function of the `System()` class to add 50 coins to the player's money total
   
   "Oh, I found 50 gold coins"
   "EXCELLENT!"
   "Now I have %[system.money]d as money"

```

#### In `Screen` with `button`, `textbutton` and `imagebutton`:
The functions in `Screen` that you want to call through a `button`, `textbutton` and `imagebutton`, are called after placing action followed by the native Re'py Function() function, and inside `( )` locate the class defined as an object divided by a `.` followed by the name of the function that you want to use, in case said function requires a parameter to be defined, these parameters must be located after the class function to be called separately by `,` between themselves.

Example:
```py
Screen earn_money:
   vbox:
      frame:
         text "$: [system.money]"
      align (0.5, 0.5)
      button text "increase money", action Function (system.earn, 50)
      textbutton "increase money" action Function (system.earn, 50)
      imagebutton idle "idle.jpg" hover "hover.jpg" action Function (system.earn, 50)
      
Label example:
   $ system = System()
   call screen earn_money      
```

### Creating the `Item()` class
In order to create each of the items required in this type, we need to establish a class that determines certain parameters which will serve us for different uses within the creation of the items that are and are not consumables.
```py
class Item:
    def __init__(self, name, cost, tag=None, effect=None):
        self.name = name 
        self.cost = cost 
        self.tag = tag 
        self.effect = effect 
        self.num = 0 
        self.amount = 0 
        self.box = 0
```
This class defines the class variables and some parameters to validate the items.

Parameters: `name`, `cost`, `tag=None`, `effect=None`

Variables:

`self.name` (`str`): Defines the name of the Item

`self.cost` (`int`): Defines the cost or purchase value

`self.tag` (`str`): Defines the group or tag of the Item to facilitate the use of its effects of both global variables (store.variable)
as in variables of the class (self.variable)
(This parameter is optional, use it only if the item to be created is of the consumable type, otherwise it is advisable not to define it)

`self.effect` (`int`): (This parameter is optional, use it only if the item to be created is of the consumable type, otherwise it is recommended not to define it)

`self.num` (`int`): Stores the total number of existing items purchased of the same name (self.name)

`self.amount` (int): Stores the amount of Items

`self.box` (`int`): Stores the total number of existing items purchased of the same name (self.name)

### Definition or Creation of the `Items`
(It is recommended to define all objects in the `label start` block within the game to avoid errors after booting or starting the game)


In order to independently define and set items (whether consumables or not) with `Item()` within the native `Ren'py` code we must declare this class as an object within a `Label` block:
```
label example:
    $Item = Item()
```
#### Common items, not cumulative or non-consumable
Common, non-accumulating or non-consumable `Items` are defined using only two parameters of the `Item()` class, for example, I want to define a set of clothes for a character 

```
label start:
    $dress = Item("Dress", 15)
```
`$dress` (`Object`): Sets the `Item` as an object which can be used later

`Item()` (`Class`): Sets the class defined above that will be used for the creation and use of the consumable `Item`

`"Dress"` (`Parameter`: `str`): Defines the Name of the `Item`

`15` (`Parameter`: `int`): Establishes the value or cost of the `Item` for which it can be acquired through the Store, subtracting it from the current value of coins acquired



#### Consumable items
The consumable `Items` are defined or created in almost the same way in which their class is established, within a `Label` block (Preferably, it is recommended to define them in the `label start` block of the game to avoid errors after booting or starting the game)
```py
label start:
    $ energize = Item("Energizing", 15, "energy", 25)
```
`$ energize` (`Object`): Sets the `Item` as an object which can be used later

`Item()` (`Class`): Sets the class defined above that will be used for the creation and use of the consumable `Item`

`"Energizing"` (`Parameter`: `str`): Defines the Name of the `Item`

`15` (`Parameter`: `int`): Establishes the value or cost of the `Item` for which it can be acquired through the Store, subtracting it from the current value of coins acquired

`"energy"` (`Parameter`: `str`): Defines the tag or group to which said `Item` belongs (This parameter is optional if the `Item` created is independent and does not belong to a set of `Items` with the same consumption purpose, in this case, the item when consumed by the player gives certain energy points to the player, therefore, it belongs to a set of 'Items' with the same purpose, to provide energy points to the player. consumed)

`25` (`Parameter`: `int`): Sets the number of effect points that the `Item` will have when consumed (For example, since it is an item that provides energy to the player, 25 will be awarded when consumed. energy points to the set total)


### Definition or Creation of other systems and their functions

#### Class
```py
class System:
   def __init__(self):
      self.money = 0
      self.total_cost = 0
      self.total_cost_num = 0
      self.items = []
      self.items_car = []
      self.items_num = []
      self.items_num_car = []
```
This class accumulates inventory, money and shopping cart data

`self.money` (`int`): Total amount of current money (if there is already an existing global variable used to set the amount of money acquired by the player, it is recommended to remove it and replace it with this class variable or replace directly `self.money` in all the lines of code found within the `System()` class by the definition `store.` followed by the name of the already existing variable that occupies this role, in case you want to show it in real time the amount of money consumed and acquired, after having defined the class `System()` as `$ system = System()` within the `Label start`, it is recommended to display in a block or line of text `[system .money]` as follows `text "$: [system.money]"`)

`self.total_cost` (`int`): Total cost amount of non-consumable items to purchase

`self.total_cost_num` (`int`): Total cost amount of consumable items to purchase

`self.items` (`List`): List of non-consumptive items already purchased

`self.items_car` (`List`): Shopping list of non-consumable items to purchase

`self.items_num` (`List`): List of consumable items already purchased

`self.items_num_car` (`List`): Shopping list of consumable items to purchase


The class as such is defined in the same way that the `Item()` class was defined, within the `Label start` block, only this one differs by not having any other additional parameters:

```py
label start:
    $ system = System()
```

#### Class functions

##### Earn or Increase Money
Adds the amount of money defined (`self.money`) by the function parameter (`amounth`) to the current money total

```py
   def earn(self, amount):
      self.money += amount
```

##### Inconsumables Store

###### Buy
Make purchases of items that are not consumables

```py
   def buy(self):
      if self.money >=  self.total_cost
         self.money -=  self.total_cost 
         self.total_cost = 0
         self.items.extend(self.items_car)
         self.items_car.clear()
         renpy.notify("Compra exitosa") 
      else:
         renpy.notify("Saldo Insuficiente") 
```
`if self.money >= self.total_cost`: Checks if the money (`self.money`) is greater than or equal (via the `>` and `=` signs together, `>=`) to the total cost payable (`self.total_cost`) using a conditional

`self.money -= self.total_cost`: Subtracts (via the `-` and `=` signs together, `-=`) the total purchase value (`self.total_cost`) with the amount of money current (`self.money`)

`self.total_cost = 0`: Restores the total cost value (`self.total_cost`) to 0 after completing the purchase

`self.items.extend(self.items_car)`: Adds the shopping cart items (`self.items`) to the inventory of non-consumable items (`self.items_car`) using the `Python` function `.extend ` to extend the shopping cart items (`self.items`) and embed the inventory of non-consumable items within it (`self.items_car`)

`self.items_car.clear()`: Removes all existing items from the shopping cart (`self.items_car`) using the `Python` function `.clear()`, which removes all data that are within the selected list (`self.items_car`)

`renpy.notify("Successful purchase")`: Notify that the Item(s) were successfully purchased and added to the inventory (self.items) of the purchase through the `renpy.notify` function

`renpy.notify("Insufficient Balance")`: Notify that there is not enough money to make the purchase through the `renpy.notify` function in case the first conditional set has returned `False`

###### Add Item to Shopping List
Add items that are not consumables to the shopping list
```py
   def add_item(self, item):
      if self.money >= self.total_cost + item.cost:
         self.total_cost += item.cost
         self.items_car.append(item) 
      else:
         renpy.notify("Saldo Insuficiente") 
```
`if self.money >= self.total_cost + item.cost`: Checks with a conditional if the total amount of money acquired (`self.money`) by the player is greater than or equal (via the `>` signs and `=` together, `>=`) than the total cost to pay (`self.total_cost`) plus (`+`) the cost of the item (`item.cost`)

`self.total_cost += item.cost`: Independently add the cost of each item (`item.cost`) to the total items to pay (`self.total_cost`)

`self.items_car.append(item)`: Adds the Item (`item`) to its corresponding shopping list (`self.items_car`) using the `Python` function `.append()`, which takes care of Add the defined values ​​to the specified list (`self.items_car`)

`renpy.notify("Insufficient Balance")`: Notify that there is not enough money to make the purchase through the `renpy.notify` function in case the first conditional set has returned `False`

###### Remove Item from Shopping List
Eliminates non-consumable items from your respective shopping list and reduces the cost of the item to the total payable
```py
   def remove_item(self, item):
      if item in self.items_car:
         self.total_cost -= item.cos
         self.items_car.remove(item) 
```
`if item in self.items_car`: Checks if the item is within its respective list

`self.total_cost -= item.cost`: Subtracts (through the `-` and `=` signs together, `-=`) individually the value of the item (`item.cost`) with the total cost to pay (`self.total_cost`)

`self.items_car.remove(item)`: Removes the value of the item (`item`) from the shopping list (`self.items_car`) permanently using the `Python` function `.remove()`, which is responsible for removing the defined values ​​from the specified list (`self.items_car`)

###### Check Item in Shopping List
Checks if the item defined by the parameter (`item`) is within the shopping list of non-consumable items (`self.items_car`) through a conditional, returns `True` if the item is within the list (`self.items_car`), otherwise it will return `False` as the item defined by the parameter (`item`) is not found.
```py
   def has_item_car(self, item):
      if item in self.items_car: 
         return True 
      else:
         return False
```

###### Check Item in Inventory
Checks if the item defined by the parameter (`item`) is within the list of acquired items (`self.items`), returns `True` if the item is within the list (`self.items`) through a conditional, otherwise it will return `False` as the item defined by the parameter (`item`) is not found.

```py
   def has_item(self, item):
      if item in self.items:
         return True 
      else:
         return False
```

##### Consumables Store

###### Buy
Make purchases of items that are consumables using `Python` functions or functions of this class (`Class System`)

```py
   def buy_items(self):
      if self.money >=  self.total_cost_num:
         self.money -=  self.total_cost_num 
         self.total_cost_num = 0
         self.items_num.extend(self.items_num_car)
         self.items_num_car.clear() 
         self.reset_items()
         renpy.notify("Successful purchase") 
      else:
         renpy.notify("Insufficient Balance")
```

`if self.money >= self.total_cost_num`: Checks if the money (`self.money`) is greater than or equal (via the `>` and `=` signs together, `>=`) to the total cost payable (`self.total_cost_num`) using a conditional

`self.money -= self.total_cost_num`: Subtracts (via the `-` and `=` signs together, `-=`) the total purchase value (`self.total_cost_num`) with the amount of money current (`self.money`)

`self.total_cost_num = 0`: Restores the total cost value (`self.total_cost_num`) to 0

`self.items_num.extend(self.items_num_car)`: Adds the shopping cart items to the consumable items inventory (`self.items_num`) using the `Python function ` `.extend` to extend the shopping cart items (`self.items_num`) and embed the inventory of non-consumable items within it (`self.items_num_car`)

`self.items_num_car.clear() `: Removes all existing items within the shopping cart (`self.items_num_car`) using the `Python` function `.clear()`

`self.reset_items()`: Use the `reset_items()` function

`renpy.notify("Successful purchase")`: Notify through the `renpy.notify` function that the Item(s) were successfully purchased and added to the consumables inventory (`self.items_num`)

`renpy.notify("Insufficient Balance")`: Notify through the `renpy.notify` function that there is not enough money to make the purchase (happens if the previous comparison returned False)

###### Restore Shopping List and Add to Inventory
NOTE:
It is MANDATORY to copy the entire following line of code according to the number of items that are defined in your project, changing `item_name` for the name of each item defined or established (`$ Item_example = Item("name_example", ... )`, here the defined item would be `Item_example`) separately and paste in `def reset_items()`:

```py
      if item_name in self.items_num: 
         item_name.num += item_name.box
         item_name.box = 0
```
Explanation:

Adds the total of items purchased to the total of items that are consumables purchased and restores the value of the items to be purchased to 0 according to the specified group or tag (in this case the item to use will be `energize`, which was defined almost at the beginning of this documentation)
```py
   def reset_items(self):
      if energize in self.items_num: 
         energize.num += energize.box
         energize.box = 0
```

`if energize in self.items_num`: Checks if the Item (in this case the specified item is `energize`) is within the purchased consumable items (`self.items_num`) using a conditional

`energize.num += energize.box`: Adds (via the `+` and `=` signs together, `+=`) the total items purchased (`energize.box`) to the total items that They are purchased consumables (`energize.num`)

`energize.box = 0`: Restores the value of the items to be purchased (`energize.box`) to 0

###### Add Item to Shopping List
Add the items that are consumables to the shopping list also using the `Python` `.append` function
```py
   def add_items(self, item):
      if self.money >= self.total_cost_num + item.cost:
         if item in self.items_num_car:
            if item.box < 99: 
               self.total_cost += item.cost
               item.box += 1  
            else: 
               renpy.notify("Maximum Achieved")
         elif not item in self.items_num_car:
            self.total_cost_num += item.cost 
            item.box += 1 
            self.items_num_car.append(item) 
      else:
         renpy.notify("Insufficient Balance")
```

`if self.money >= self.total_cost_num + item.cost`: Checks with a conditional if the total amount of money acquired (`self.money`) by the player is greater than or equal (through the `>` signs and `=` together, `>=`) than the total cost to pay (`self.total_cost_num`) plus (`+`) the cost of the item (`item.cost`)

`if item in self.items_num_car`: Checks if the Item is found within the shopping list (`self.items_num_car`) using a conditional 

`if item.box < 99`: Sets a maximum item limit (`99`) for each item purchased

`self.total_cost += item.cost`: Add (through the `+` and `=` signs together, `+=`) independently the cost of each item (`item.cost`) to the total items to pay (`self.total_cost`)

`item.box += 1`: Adds a consumable item to the total number of items of the same type to be purchased

`renpy.notify("Maximum Reached")`: Notify that the maximum limit has been reached through the `renpy.notify` function

`elif not item in self.items_num_car`: Checks with a conditional if the item is not found in the shopping list (`self.items_num_car`)

`self.total_cost_num += item.cost`: Add (through the `+` and `=` signs together, `+=`) independently the cost of each item to the total items to be paid

`item.box += 1`: Adds (through the `+` and `=` signs together, `+=`) a consumable item (`1`) to the total number of items of the same type to be purchased (`item .box`)

`self.items_num_car.append(item)`: Adds the Item (`item`) to its corresponding shopping list (`self.items_num_car`) using the `Python` function `.append()`, which takes care of Add the defined values ​​to the specified list (`self.items_num_car`)

`renpy.notify("Insufficient Balance")`: Notify that there is not enough money to make the purchase through the `renpy.notify` function in case the first conditional set has returned `False`

###### Reduce total accumulated Items in the Shopping List
Subtract the total value of the consumable items from their respective shopping list by 1 according to the type or name of the consumable.
```py
   def remove_items(self, item):
      if item in self.items_num_car:
         if item.box > 1: 
            self.total_cost_num -= item.cost
            item.box -= 1 
         else:
            self.total_cost -= item.cost
            item.box = 0 
            self.items_num_car.remove(item) 
```

`if item in self.items_num_car`: Checks if the item is in your shopping list (`self.items_num_car`) using a conditional

`if item.box > 1`: Sets a minimum limit of items to purchase (`item.box`) within the shopping list (it is recommended not to change this value, otherwise the system could work incorrectly)

`self.total_cost_num -= item.cost`: Subtracts (through the `-` and `=` signs together, `-=`) individually the value of the item (`item.cost`) from the total cost to pay ( `self.total_cost_num`)

`item.box -= 1`: Subtracts (through the `-` and `=` signs together, `-=`) the number of items to buy (`item.box`) individually

`self.total_cost -= item.cost`: Subtracts (through the `-` and `=` signs together, `-=`) individually the value (`item.cost`) of the item from the total cost to pay ( `self.total_cost`)

`item.box = 0`: Restores the number of items to buy (`item.box`) to 0

`self.items_num_car.remove(item)`: Removes the value of the item (`item`) from the shopping list (`self.items_car`) permanently using the `Python` function `.remove()`, which is responsible for removing the defined values ​​from the specified list (`self.items_num_car`)

###### Remove Item from Shopping List
Removes and restores item values ​​in the shopping list for items that are consumables
```py
   def delete_items(self, item):
      if item in self.items_num_car: 
         self.total_cost_num -= item.cost * item.box
         item.box = 0
         self.items_num_car.remove(item)
```

`if item in self.items_num_car`: Checks if the item is in your shopping list (`self.items_num_car`) using a conditional

`if item.box > 1`: Sets a minimum limit of items to purchase (`item.box`) within the shopping list (it is recommended not to change this value, otherwise the system could work incorrectly)

`self.total_cost_num -= item.cost`: Subtracts (through the `-` and `=` signs together, `-=`) individually the value of the item (`item.cost`) from the total cost to pay ( `self.total_cost_num`)

`item.box -= 1`: Subtracts (through the `-` and `=` signs together, `-=`) the number of items to buy (`item.box`) individually

`self.total_cost -= item.cost`: Subtracts (through the `-` and `=` signs together, `-=`) individually the value (`item.cost`) of the item from the total cost to pay ( `self.total_cost`)

`item.box = 0`: Restores the number of items to buy (`item.box`) to 0

`self.items_num_car.remove(item)`: Removes the value of the item (`item`) from the shopping list (`self.items_car`) permanently using the `Python` function `.remove()`, which is responsible for removing the defined values ​​from the specified list (`self.items_num_car`)

###### Remove Item from Shopping List
Removes and restores item values ​​in the shopping list for items that are consumables
```py
   def not_use(self, item):
      if item in self.items_num: 
         if item.num - item.amount + 1 <= item.num:
            item.amount -= 1
         elif item.num - item.amount == item.num: 
            renpy.notify("Maximum Achieved") 
```

`if item in self.items_num`: Checks if the Item is within the purchased consumable items (`self.items_num`) using a conditional

`if item.num - item.amount + 1 <= item.num`: Checks using a conditional if the number of items (`item.num`) minus (`-`) the total to consume (`item.amount` ) plus (`+`) 1 is less than or equal (via the `<` and `=` signs together, `<=`) to the current total items (`item.num`)

`item.amount -= 1`: Subtracts (via the `-` and `=` signs together, `-=`) the number of items to use (`item.amount`) individually

`elif item.num - item.amount == item.num`: Checks if the number of items (`item.num`) minus (`-`) the total to consume (`item.amount`) is equal to the total of current items (item.num)

`renpy.notify("Maximum Reached")`: Notify through the `renpy.notify` function that the maximum limit has been reached 


###### Increase Items to Consume
NOTE:
It is MANDATORY to copy the entire following line of code, changing `tag_name`, `variable_name` and `max_limit_effect` for the name of each tag or label, variable defined or established for the effect of the item and the maximum number of points established for the effect of said item separately and paste in `def effect_use()`
```py
   def use(self, item):
      if item in self.items_num: 
         if item.tag == "tag_name":
            if store.variable_name + item.effect*item.amount < max_limit_effect and item.num - item.amount -1 > 0: 
               item.amount += 1
            else:
               renpy.notify(f"cannot be used {item.name}") 
```
Explanation:

Add the items that are consumables to the total to consume
```py
   def use(self, item):
      if item in self.items_num: 
         if item.tag == "energy":
            if store.energy + item.effect*item.amount < 100 and item.num - item.amount -1 > 0: 
               item.amount += 1
            else:
               renpy.notify(f"cannot be used {item.name}") 
```

`if item in self.items_num`: Checks if the Item is within the purchased consumable items (`self.items_num`)

`if item.tag == "energy"`: Checks if the item belongs to a specific group or tag (in this case, `"energy"`) using its class variable (`item.tag`) through a conditional

`if store.energy + item.effect*item.amount < 100 and item.num - item.amount -1 > 0`: Using a conditional, establish a limit of the effect (in this case it is `100`) and check if the value of the variable (`store.energy`) plus (`+`) the value of the item's effect (`item.effect`) multiplied (`*`) by the amount of items to consume (`item.amount` ) is less than `100` (that is, the limit, this digit can be changed according to the need or maximum limit of an effect) and if the total quantity of the item acquired (`item.num`) less (`-` ) the total amount to consume (`item.amount`) less (`-`) is greater than (`>`) `0`

`item.amount += 1`: Adds (via the `+` and `=` signs together, `+=`) the number of items to use (`item.amount`) individually

`renpy.notify(f"Cannot use {item.name}")`: Notify that the item can no longer be used or can no longer be used through the `renpy.notify` function also using `{item .name}` inside a `str` (happens if the previous conditional returned False)

###### Consume Selected Item
Consume and execute the effects of consumable items
```py
   def consume(self, item):
      if item in self.items_num: 
         if item.num - item.amount >= 0:
            self.effect_use(item) 
            item.num -= item.amount

            item.amount = 0  
            if item.num == 0:   
               self.items_num.remove(item)
            renpy.notify(f"It was used {item.name}")
```
`if item in self.items_num`: Checks if the Item is among the consumable items acquired through a conditional

`if item.num - item.amount >= 0`: Checks if the total amount of items purchased (`item.num`) minus the items to be consumed (`item.amount`) is equal to or greater than 0

`self.effect_use(item)`: Use the effect_use() function together with the `item` parameter defined in the current function

`item.num -= item.amount`: Subtracts the number of items to be consumed (`item.amount`) from the total number of items purchased (`item.num`)

`item.amount = 0`: Restores the amount of items to consume (`item.amount) to 0 after having consumed said item and subtracting the total acquired

`if item.num == 0`: Checks if the total number of items (`item.num`) is equal to 0 through a conditional

`self.items_num.remove(item)` : Use the native `Python` function `.remove` to remove the `item` from the total items acquired if the previous conditional returned `True`

`renpy.notify(f"{item.name} was used")`: Notify that the item was consumed via the `renpy.notify` function also using `{item.name}` inside a `str` to be able to acquire the name of the `item` used successfully

###### Item Effects in Point System
NOTE:
It is MANDATORY to copy the entire following line of code according to the number of tags or labels that are defined in your item sets, changing `tag_name`, `variable_name` and `limit_max_effect` for the name of each tag or label, the variable defined or established and the maximum limit of the effect in score separately and paste in `def effect_use()`:
```py
      if item.tag == "tag_name":
         store.energy += item.effect*item.amount 
         if store.variable_name > limit_max_effect:
            store.variable_name = limit_max_effect 
```
Explanation:

Consume and execute the effects of items that are consumable, at the same time defining a point limit without exceeding it (it is recommended to change)
```py
   def effect_use(self, item):
      if item.tag == "energy":
         store.energy += item.effect*item.amount 
         if store.energy > 100:
            store.energy = 100 
```
`if item.tag == "energy"`: Checks through a conditional if the item belongs to a group or tag (`item.tag`) (this makes it easier to use the effects in general) at the same time in the which determines if the group or tag of the item is specifically defined, in this case, we want to verify if the `item.tag` is `"energy"`

`store.energy += item.effect*item.amount`: Multiplies (via the `*` sign or symbol) the effect (called via `item.effect`) by the amount of items consumed (called by ` item.amount`) and adds them (In this case) to the player's total energy within a point system defined or established by a global variable which is called with `store.` followed by the name of said variable (in this case `store.energy`)

`if store.energy > 100`: Establishes a limit for the energy variable defined at the beginning, this limit will be of type `int`, in this case the limit number specified is `100` (Both the limit and the variable after `store.` can be replaced), If the established limit is exceeded, the variable is restored to the defined limit `store.energy = 100` (here the same number or quantity must be placed that was previously placed in the conditional)

###### Check Item in Shopping List
Checks if the item defined by the parameter is within the shopping list of consumable items, returns `True` if the item is within the list, otherwise it will return `False` if said item defined by the parameter is not found
```py
   def has_items_car(self, item):
      if item in self.items_num_car: 
         return True 
      else:
         return False
```
   
###### Check Item in Inventory
Checks if the item defined by the parameter is within the list of purchased consumable items, returns `True` if the item is within the list, otherwise it will return `False` if said item defined by the parameter is not found.
```py
   def has_items(self, item):
      if item in self.items_num:
         return True
      else:
         return False
```