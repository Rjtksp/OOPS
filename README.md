# OOPS
Let's build a Cake Shop CRUD System using the cake analogy. Every single line will relate to cakes!


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
echo "🍽️  LET'S EAT!\n";
echo $chocolateCake->cutSlice() . "\n";
echo $chocolateCake->cutSlice() . "\n";
?>
