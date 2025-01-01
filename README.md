//# DietSuggestionBasedOnJava
import java.util.Scanner;

public class PersonalDietPlanner {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        // User input for name
        System.out.print("Enter Your Name: ");
        String Name = input.nextLine();

        // User input for gender with validation
        System.out.print("Enter Your Gender (male/female): ");
        String Gender = input.nextLine().trim().toLowerCase();
        while (!Gender.equals("male") && !Gender.equals("female")) {
            System.out.print("Invalid input. Please enter 'male' or 'female': ");
            Gender = input.nextLine().trim().toLowerCase();
        }

        // User input for age
        System.out.print("Enter Your Age: ");
        int Age = input.nextInt();

        // User input for height with validation
        System.out.print("Enter Your Height [Cm]: ");
        double Height = input.nextDouble();
        while (Height <= 0) {
            System.out.print("Invalid height. Please enter a positive value: ");
            Height = input.nextDouble();
        }

        // User input for weight with validation
        System.out.print("Enter Your Weight [Kg]: ");
        double Weight = input.nextDouble();
        while (Weight <= 0) {
            System.out.print("Invalid weight. Please enter a positive value: ");
            Weight = input.nextDouble();
        }

        // User input for physical activity level with validation
        System.out.println("\nSelect Your Physical Activity Level In A Scale Of 1-5 Mentioned Below:");
        System.out.println("1 = Sedentary");
        System.out.println("2 = Lightly active");
        System.out.println("3 = Moderately active");
        System.out.println("4 = Active");
        System.out.println("5 = Very active");
        System.out.print("\nEnter: ");
        int Activity = input.nextInt();
        while (Activity < 1 || Activity > 5) {
            System.out.print("Invalid input. Please enter a value between 1 and 5: ");
            Activity = input.nextInt();
        }
        System.out.println();

        double ActiveLevel = ActiveLevelCounter(Activity);

        // BMI calculation
        double BMI = Double.parseDouble(BMIcalculator(Height, Weight));

        // Calorie calculation
        int CalorieCount = Calories(Gender, Age, Height, Weight, ActiveLevel);

        // Generate and display diet plan
        DietPlanner(Name, CalorieCount, BMI);

        input.close();
    }

    private static double ActiveLevelCounter(int Activity) {
        // Determine activity level multiplier
        double Counter = 1; // Default
        switch (Activity) {
            case 1 -> Counter = 1.2;    // Sedentary
            case 2 -> Counter = 1.375; // Lightly active
            case 3 -> Counter = 1.55;  // Moderately active
            case 4 -> Counter = 1.725; // Active
            case 5 -> Counter = 1.9;   // Very active
        }
        return Counter;
    }

    static String BMIcalculator(double Height, double Weight) {
        // Calculate BMI
        double BMIValue = Weight / (Height * Height) * 10000;
        return String.format("%.2f", BMIValue);
    }

    static int Calories(String Gender, int Age, double Height, double Weight, double ActiveLevel) {
        // Calculate calorie requirements based on gender and activity level
        if (Gender.equals("male")) {
            return (int) ((10 * Weight + 6.25 * Height - (5 * Age) + 5) * ActiveLevel);
        } else {
            return (int) ((10 * Weight + 6.25 * Height - (5 * Age) - 161) * ActiveLevel);
        }
    }

    static void DietPlanner(String Name, int CalorieCount, double BMI) {
        // Display diet plan
        System.out.println("Hi " + Name + ", Here Is Your Diet Planner\n");
        System.out.println("Your Daily Required Calories: " + CalorieCount + "\n");

        // Calculate macronutrient ranges
        double MinCarbs = ((CalorieCount * 0.55) / 4);
        double MaxCarbs = ((CalorieCount * 0.60) / 4);
        double MinProtein = ((CalorieCount * 0.25) / 4);
        double MaxProtein = ((CalorieCount * 0.30) / 4);
        double MinFats = ((CalorieCount * 0.15) / 9);
        double MaxFats = ((CalorieCount * 0.20) / 9);

        // Display macronutrient recommendations
        System.out.printf("Carbohydrates (Grams): %.2f - %.2f grams%n", MinCarbs, MaxCarbs);
        System.out.printf("Protein (Grams): %.2f - %.2f grams%n", MinProtein, MaxProtein);
        System.out.printf("Fats (Grams): %.2f - %.2f grams%n%n", MinFats, MaxFats);

        // Weight management recommendations
        int MildWeightLossCalories = CalorieCount - 250;
        int WeightLossCalories = CalorieCount - 500;
        int ExtremeWeightLossCalories = CalorieCount - 1000;
        int MildWeightGainCalories = CalorieCount + 250;
        int WeightGainCalories = CalorieCount + 500;
        int FastWeightGainCalories = CalorieCount + 1000;

        System.out.println("Normal Weight BMI Range: 18.5 - 24.9");
        System.out.println("Your BMI Value: " + BMI + "\n");

        if (BMI < 18.5) {
            System.out.println("You Are Underweight. You Need To Gain Weight.\n");
            System.out.println("Recommended Foods:");
            System.out.println("- Whole grains (oats, quinoa, brown rice)");
            System.out.println("- High-calorie fruits (bananas, avocados, dried fruits)");
            System.out.println("- Healthy fats (nuts, seeds, peanut butter)");
            System.out.println("- Protein-rich foods (chicken, eggs, fish, legumes)");
            System.out.println("- Dairy products (whole milk, yogurt, cheese)\n");
        } else if (BMI >= 18.5 && BMI <= 24.9) {
            System.out.println("You Are Normal Weight. Keep maintaining your healthy weight.\n");
            System.out.println("Recommended Foods:");
            System.out.println("- Balanced diet with lean proteins, whole grains, fruits, and vegetables");
            System.out.println("- Healthy snacks (nuts, seeds, yogurt)");
            System.out.println("- Low-fat dairy and healthy oils (olive oil, canola oil)\n");
        } else if (BMI >= 25 && BMI <= 29.9) {
            System.out.println("You Are Overweight. You Need To Lose Weight.\n");
            System.out.println("Recommended Foods:");
            System.out.println("- High-fiber foods (leafy greens, broccoli, lentils)");
            System.out.println("- Lean protein (chicken breast, turkey, fish)");
            System.out.println("- Low-calorie snacks (carrot sticks, cucumber, air-popped popcorn)");
            System.out.println("- Avoid processed and sugary foods\n");
        } else {
            System.out.println("You Are Obese. You Need To Lose Significant Weight.\n");
            System.out.println("Recommended Foods:");
            System.out.println("- Low-carb vegetables (spinach, kale, zucchini)");
            System.out.println("- Lean protein and legumes");
            System.out.println("- Whole grains in moderation");
            System.out.println("- Avoid high-calorie and processed foods entirely\n");
        }

        System.out.println("\nStay Hydrated, Hit the Gym, and All The Best!");
        System.out.println("Thank You.");
    }
}
