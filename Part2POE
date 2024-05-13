using System;

class Program
{
    static void Main(string[] args)
    {
        RecipeManager recipeManager = new RecipeManager();

        Console.WriteLine("Welcome to Recipe Maker!");

        while (true)
        {
            Console.WriteLine("\nMain Menu:");
            Console.WriteLine("1. Add Recipe");
            Console.WriteLine("2. Display All Recipes");
            Console.WriteLine("3. Exit");

            Console.Write("Enter your choice: ");
            string choiceStr = Console.ReadLine();

            if (!int.TryParse(choiceStr, out int choice))
            {
                Console.WriteLine("Invalid choice! Please enter a number.");
                continue;
            }

            switch (choice)
            {
                case 1:
                    recipeManager.AddRecipe();
                    break;
                case 2:
                    recipeManager.DisplayAllRecipes();
                    break;
                case 3:
                    Console.WriteLine("Thank you for using Recipe Maker!");
                    return;
                default:
                    Console.WriteLine("Invalid choice!");
                    break;
            }
        }
    }
}

class RecipeManager
{
    private List<Recipe> recipes = new List<Recipe>();

    public void AddRecipe()
    {
        Recipe recipe = new Recipe();

        Console.Write("Enter recipe name: ");
        recipe.Name = Console.ReadLine();

        recipe.AddIngredients();
        recipe.AddSteps();

        // Subscribing to the event
        recipe.RecipeExceedsCalories += NotifyUserRecipeExceedsCalories;

        recipes.Add(recipe);

        Console.WriteLine("Recipe added successfully!");
    }

    public void DisplayAllRecipes()
    {
        if (recipes.Count == 0)
        {
            Console.WriteLine("No recipes found.");
            return;
        }

        recipes.Sort((x, y) => string.Compare(x.Name, y.Name));

        Console.WriteLine("\nRecipes:");
        foreach (Recipe recipe in recipes)
        {
            Console.WriteLine($"- {recipe.Name}");
        }

        Console.Write("Enter the name of the recipe to display: ");
        string recipeName = Console.ReadLine();

        Recipe selectedRecipe = recipes.Find(r => r.Name.Equals(recipeName, StringComparison.OrdinalIgnoreCase));
        if (selectedRecipe != null)
        {
            selectedRecipe.DisplayRecipe();
        }
        else
        {
            Console.WriteLine("Recipe not found.");
        }
    }

    // Delegate method to notify user when a recipe exceeds 300 calories
    private void NotifyUserRecipeExceedsCalories(Recipe recipe)
    {
        Console.WriteLine($"Warning: Recipe '{recipe.Name}' exceeds 300 calories!");
    }
}

class Recipe
{
    public string Name { get; set; }
    private List<Ingredient> ingredients = new List<Ingredient>();
    private List<string> steps = new List<string>();

    // Event to notify when recipe exceeds 300 calories
    public delegate void RecipeCaloriesExceededHandler(Recipe recipe);
    public event RecipeCaloriesExceededHandler RecipeExceedsCalories;

    public void AddIngredients()
    {
        Console.Write("Enter the number of ingredients: ");
        int numIngredients = int.Parse(Console.ReadLine());

        for (int i = 0; i < numIngredients; i++)
        {
            Console.Write($"Enter name of ingredient #{i + 1}: ");
            string name = Console.ReadLine();

            Console.Write($"Enter quantity of {name}: ");
            double quantity = double.Parse(Console.ReadLine());

            Console.Write($"Enter unit for {name}: ");
            string unit = Console.ReadLine();

            Console.Write($"Enter number of calories for {name}: ");
            int calories = int.Parse(Console.ReadLine());

            Console.Write($"Enter food group for {name}: ");
            string foodGroup = Console.ReadLine();

            ingredients.Add(new Ingredient(name, quantity, unit, calories, foodGroup));
        }
    }

    public void AddSteps()
    {
        Console.Write("Enter the number of steps: ");
        int numSteps = int.Parse(Console.ReadLine());

        for (int i = 0; i < numSteps; i++)
        {
            Console.Write($"Enter step #{i + 1}: ");
            steps.Add(Console.ReadLine());
        }
    }

    public void DisplayRecipe()
    {
        Console.WriteLine($"\nRecipe: {Name}");

        Console.WriteLine("Ingredients:");
        foreach (Ingredient ingredient in ingredients)
        {
            Console.WriteLine($"{ingredient.Quantity} {ingredient.Unit} of {ingredient.Name}");
        }

        Console.WriteLine("\nSteps:");
        for (int i = 0; i < steps.Count; i++)
        {
            Console.WriteLine($"{i + 1}. {steps[i]}");
        }

        int totalCalories = CalculateTotalCalories();
        Console.WriteLine($"Total Calories: {totalCalories}");

        if (totalCalories > 300)
        {
            // Triggering the event
            RecipeExceedsCalories?.Invoke(this);
        }
    }

    private int CalculateTotalCalories()
    {
        int totalCalories = 0;
        foreach (Ingredient ingredient in ingredients)
        {
            totalCalories += ingredient.Calories;
        }
        return totalCalories;
    }
}

class Ingredient
{
    public string Name { get; set; }
    public double Quantity { get; set; }
    public string Unit { get; set; }
    public int Calories { get; set; } 
    public string FoodGroup { get; set; }

    public Ingredient(string name, double quantity, string unit, int calories, string foodGroup)
    {
        Name = name;
        Quantity = quantity;
        Unit = unit;
        Calories = calories;
        FoodGroup = foodGroup;
    }
}
