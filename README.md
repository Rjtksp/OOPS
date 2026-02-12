# OOPS
Let's build a Cake Shop CRUD System using the cake analogy. Every single line will relate to cakes!

### Think of OOP LIKE THIS:
```

CLASS = A blueprint (like a cake recipe)
OBJECT = The actual cake (made from the recipe)
PROPERTIES = Ingredients (flour, eggs, sugar)
METHODS = Actions (bake, frost, cut)
CONSTRUCTOR = "Preheat oven" (happens first)
$this       = "This cake" (the current one)

```

```
<?php
// ============ THE BLUEPRINT (CLASS) ============
class Cake {
    // INGREDIENTS (PROPERTIES)
    public $flavor;      // chocolate, vanilla, etc
    public $size;        // small, medium, large
    public $frosting;    // buttercream, cream cheese, etc
    public $slicesLeft;  // how many slices remain
    
    // OVEN SETTINGS (CONSTRUCTOR - runs when you make a cake)
    public function __construct($flavor, $size, $frosting) {
        $this->flavor = $flavor;
        $this->size = $size;
        $this->frosting = $frosting;
        
        // Different sizes have different slices
        if($size == "small") $this->slicesLeft = 4;
        if($size == "medium") $this->slicesLeft = 8;
        if($size == "large") $this->slicesLeft = 12;
    }
    
    // ACTIONS (METHODS) - what the cake CAN DO
    public function bake() {
        return "Baking a {$this->flavor} cake with {$this->frosting} frosting!";
    }
    
    public function cutSlice() {
        if($this->slicesLeft > 0) {
            $this->slicesLeft--;
            return "Cut a slice! {$this->slicesLeft} slices left.";
        } else {
            return "No slices left! Cake is finished!";
        }
    }
    
    public function addFrosting($newFrosting) {
        $this->frosting = $newFrosting;
        return "Added {$newFrosting} frosting!";
    }
    
    public function cakeInfo() {
        return "{$this->flavor} cake, {$this->size}, {$this->frosting} frosting, {$this->slicesLeft} slices";
    }
}

// ============ MAKING ACTUAL CAKES (OBJECTS) ============
echo "LET'S BAKE SOME CAKES!\n";
echo str_repeat("=", 50) . "\n\n";

// OBJECT 1: Chocolate cake
$chocolateCake = new Cake("chocolate", "large", "buttercream");
echo $chocolateCake->bake() . "\n";
echo $chocolateCake->cakeInfo() . "\n";

echo "\n";

// OBJECT 2: Vanilla cake
$vanillaCake = new Cake("vanilla", "medium", "cream cheese");
echo $vanillaCake->bake() . "\n";
echo $vanillaCake->cakeInfo() . "\n";

echo "\n";
echo "LET'S EAT!\n";
echo $chocolateCake->cutSlice() . "\n";
echo $chocolateCake->cutSlice() . "\n";
?>
```

### SEE? One blueprint made 2 different cakes! Each cake has its own flavor, size, and slices!

### PART 2: CAKE SHOP CRUD SYSTEM
```
<?php
// ============ STEP 1: CAKE BLUEPRINT ============
class Cake {
    // INGREDIENTS (PROPERTIES)
    public $id;
    public $flavor;
    public $size;
    public $frosting;
    public $price;
    public $slicesLeft;
    
    // BAKE THE CAKE (CONSTRUCTOR)
    public function __construct($flavor, $size, $frosting, $price) {
        $this->id = uniqid('cake_');  // Each cake gets unique ID
        $this->flavor = $flavor;
        $this->size = $size;
        $this->frosting = $frosting;
        $this->price = $price;
        
        // Set slices based on size
        $slicesMap = [
            'small' => 4,
            'medium' => 8,
            'large' => 12,
            'party' => 20
        ];
        $this->slicesLeft = $slicesMap[$size] ?? 8;
    }
    
    // CAKE ACTIONS (METHODS)
    public function getLabel() {
        return "{$this->flavor} ({$this->size}) - \${$this->price}";
    }
    
    public function getFullDetails() {
        return "{$this->flavor} | {$this->size} | {$this->frosting} | \${$this->price} | {$this->slicesLeft} slices";
    }
}

// ============ STEP 2: CAKE SHOP (CRUD SYSTEM) ============
class CakeShop {
    // CAKE DISPLAY CASE (holds all cakes)
    private $cakes = [];
    
    // C = CREATE (Bake new cake)
    public function bakeCake($flavor, $size, $frosting, $price) {
        $cake = new Cake($flavor, $size, $frosting, $price);
        $this->cakes[$cake->id] = $cake;
        echo "BAKED: " . $cake->getFullDetails() . "\n";
        return $cake;
    }
    
    // R = READ (Show menu - all cakes)
    public function showMenu() {
        if(empty($this->cakes)) {
            echo "No cakes in display case!\n";
            return;
        }
        
        echo "\n CAKE SHOP MENU \n";
        echo str_repeat("-", 50) . "\n";
        
        $counter = 1;
        foreach($this->cakes as $cake) {
            echo $counter . ". " . $cake->getFullDetails() . "\n";
            $counter++;
        }
        echo str_repeat("-", 50) . "\n";
    }
    
    // R = READ (Find one cake by ID)
    public function findCake($id) {
        return $this->cakes[$id] ?? null;
    }
    
    // U = UPDATE (Change frosting)
    public function changeFrosting($id, $newFrosting) {
        $cake = $this->findCake($id);
        if($cake) {
            $oldFrosting = $cake->frosting;
            $cake->frosting = $newFrosting;
            echo "Updated: {$cake->flavor} cake - {$oldFrosting} → {$newFrosting}\n";
            return true;
        }
        echo "Cake not found!\n";
        return false;
    }
    
    // U = UPDATE (Change price)
    public function updatePrice($id, $newPrice) {
        $cake = $this->findCake($id);
        if($cake) {
            $oldPrice = $cake->price;
            $cake->price = $newPrice;
            echo "Price updated: \${$oldPrice} → \${$newPrice}\n";
            return true;
        }
        return false;
    }
    
    // D = DELETE (Sell out/remove cake)
    public function sellCake($id) {
        if(isset($this->cakes[$id])) {
            $flavor = $this->cakes[$id]->flavor;
            unset($this->cakes[$id]);
            echo "SOLD OUT: {$flavor} cake (all slices sold!)\n";
            return true;
        }
        return false;
    }
    
    // EXTRA: Buy a slice
    public function buySlice($id) {
        $cake = $this->findCake($id);
        if($cake) {
            if($cake->slicesLeft > 0) {
                $cake->slicesLeft--;
                echo "Bought 1 slice! {$cake->slicesLeft} slices of {$cake->flavor} left\n";
                return true;
            } else {
                echo "No slices left of {$cake->flavor} cake!\n";
                return false;
            }
        }
        return false;
    }
}

// ============ STEP 3: OPEN OUR CAKE SHOP! ============

echo "\n WELCOME TO OOP CAKE SHOP \n";
echo "========================================\n\n";

$shop = new CakeShop();

// CREATE - Bake some cakes!
echo "BAKING CAKES:\n";
echo "----------------\n";
$cake1 = $shop->bakeCake("Chocolate", "large", "buttercream", 25.99);
$cake2 = $shop->bakeCake("Vanilla", "medium", "cream cheese", 19.99);
$cake3 = $shop->bakeCake("Red Velvet", "small", "cream cheese", 15.99);
$cake4 = $shop->bakeCake("Lemon", "medium", "lemon buttercream", 21.99);

// READ - Show menu
$shop->showMenu();

// UPDATE - Change some things
echo "\n UPDATING CAKES:\n";
echo "------------------\n";
$shop->changeFrosting($cake1->id, "chocolate ganache");
$shop->updatePrice($cake2->id, 17.99);

// READ - Show updated menu
$shop->showMenu();

// BUY SOME SLICES
echo "\n CUSTOMER ORDERS:\n";
echo "------------------\n";
$shop->buySlice($cake1->id);
$shop->buySlice($cake1->id);
$shop->buySlice($cake2->id);

// READ - Show menu after slices bought
$shop->showMenu();

// DELETE - Cake sold out
echo "\n END OF DAY:\n";
echo "--------------\n";
$shop->sellCake($cake3->id);

// READ - Final menu
$shop->showMenu();

// BONUS: Customer asks for cake by flavor
echo "\n CUSTOMER SEARCH:\n";
echo "------------------\n";
foreach($shop->findCake($cake1->id) as $id => $cake) {
    echo "Found: " . $cake->getFullDetails() . "\n";
}
?>
```

### PART 3: OOP CONCEPTS WITH CAKES (Super Simple Way)
### 1. ENCAPSULATION (Private Recipe)
```
class SecretRecipeCake {
    private $secretIngredient = "love";  // PRIVATE - can't see outside!
    public $flavor;
    
    public function __construct($flavor) {
        $this->flavor = $flavor;
    }
    
    // Only way to access secret ingredient
    public function getSecret() {
        return "Secret ingredient: " . $this->secretIngredient;
    }
}

$cake = new SecretRecipeCake("Chocolate");
echo $cake->flavor;        // Works - public
// echo $cake->secretIngredient;  // ERROR! Private!
echo $cake->getSecret();   // Works - public method
```

### 2. INHERITANCE (Family Recipes)
```
// PARENT CLASS - Basic Cake
class BasicCake {
    public $flavor;
    
    public function __construct($flavor) {
        $this->flavor = $flavor;
    }
    
    public function bake() {
        return "Baking a {$this->flavor} cake at 350°F";
    }
}

// CHILD CLASS - Birthday Cake (is a BasicCake + more)
class BirthdayCake extends BasicCake {
    public $candles;
    public $message;
    
    public function addCandles($number) {
        $this->candles = $number;
        return "Added {$number} candles!";
    }
    
    public function writeMessage($msg) {
        $this->message = $msg;
        return "Written: '{$msg}'";
    }
}

$birthday = new BirthdayCake("Chocolate");
echo $birthday->bake();          // From parent
echo $birthday->addCandles(5);   // From child
```

### 3. POLYMORPHISM (Same word, different cakes)
```
class WeddingCake extends BasicCake {
    public function decorate() {
        return "Adding elegant white flowers";
    }
}

class KidsCake extends BasicCake {
    public function decorate() {
        return "Adding colorful cartoon characters";
    }
}

class FancyCake extends BasicCake {
    public function decorate() {
        return "Adding gold leaf and chocolate curls";
    }
}

// SAME METHOD NAME - DIFFERENT RESULTS!
$cakes = [
    new WeddingCake("Vanilla"),
    new KidsCake("Chocolate"),
    new FancyCake("Red Velvet")
];

foreach($cakes as $cake) {
    echo $cake->decorate() . "\n";
}
// Output:
// Adding elegant white flowers
// Adding colorful cartoon characters
// Adding gold leaf and chocolate curls
```

### PART 4: CRUD - Just 4 Actions!
```
CAKE SHOP CRUD:

CREATE      = Bake a new cake
READ        = Look at menu / Find a cake
UPDATE      = Change frosting / Change price
DELETE      = Cake sold out / Remove from menu

THAT'S IT! JUST 4 ACTIONS!
```

### REMEMBER THIS CHART:
```
| OOP Term          | Cake Analogy                        | Code Example |
|-------------------|-------------------------------------|--------------|
| **CLASS**         | Cake recipe / blueprint             | `class Cake { }` |
| **OBJECT**        | Actual cake you can eat             | `$myCake = new Cake();` |
| **PROPERTY**      | Ingredients                         | `$cake->flavor = "chocolate";` |
| **METHOD**        | Actions (bake, cut, frost)          | `$cake->cutSlice();` |
| **CONSTRUCTOR**   | Preheat oven (runs first)           | `public function __construct() { }` |
| **$this**         | "This cake right here"              | `$this->flavor` |
| **CREATE**        | Bake new cake                       | `$shop->bakeCake()` |
| **READ**          | Look at cake menu                   | `$shop->showMenu()` |
| **UPDATE**        | Change frosting                     | `$shop->changeFrosting()` |
| **DELETE**        | Cake sold out                       | `$shop->sellCake()` |
```


### YOUR TURN - Build This Simple CRUD: Pizza (Not cake, but still food!)
```
<?php
class Pizza {
    public $topping;
    public $size;
    public $slices;
    
    public function __construct($topping, $size) {
        $this->topping = $topping;
        $this->size = $size;
        $this->slices = ($size == "large") ? 8 : 6;
    }
}

class Pizzeria {
    private $pizzas = [];
    
    // TODO 1: CREATE - Make pizza
    public function makePizza($topping, $size) {
        // Your code here
    }
    
    // TODO 2: READ - Show menu
    public function showMenu() {
        // Your code here
    }
    
    // TODO 3: UPDATE - Add extra cheese
    public function addExtraCheese($id) {
        // Your code here
    }
    
    // TODO 4: DELETE - Pizza finished
    public function pizzaFinished($id) {
        // Your code here
    }
}

// TEST IT:
$pizzeria = new Pizzeria();
$pizzeria->makePizza("Pepperoni", "large");
$pizzeria->makePizza("Mushroom", "medium");
$pizzeria->showMenu();
// ... try the other methods!
?>
```
