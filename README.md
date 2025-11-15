using System;
using System.IO;

/// <summary>
/// Contains methods for recursive mathematical calculations.
/// </summary>
public class MathFunctions
{
    /// <summary>
    /// Calculates the factorial of a non-negative integer recursively.
    /// </summary>
    /// <param name="n">The number to calculate the factorial of.</param>
    /// <returns>The factorial of n.</returns>
    /// <exception cref="ArgumentOutOfRangeException">Thrown if n is negative or too large for long.</exception>
    public static long Factorial(int n)
    {
        if (n < 0)
        {
            throw new ArgumentOutOfRangeException(nameof(n), "Factorial is not defined for negative numbers.");
        }
        // Practical check to prevent immediate overflow for the long type. Max n is 20 for long.
        if (n > 20) 
        {
            throw new ArgumentOutOfRangeException(nameof(n), $"Input ({n}) is too large. Factorial calculation will overflow the 'long' data type.");
        }

        if (n == 0)
        {
            return 1;
        }

        return n * Factorial(n - 1);
    }

    /// <summary>
    /// Calculates the nth Fibonacci number recursively.
    /// </summary>
    /// <param name="n">The position in the Fibonacci sequence (n >= 0).</param>
    /// <returns>The nth Fibonacci number.</returns>
    /// <exception cref="ArgumentOutOfRangeException">Thrown if n is negative.</exception>
    public static int Fibonacci(int n)
    {
        if (n < 0)
        {
            throw new ArgumentOutOfRangeException(nameof(n), "Fibonacci sequence is defined for non-negative integers.");
        }

        if (n == 0)
        {
            return 0;
        }
        if (n == 1)
        {
            return 1;
        }

        return Fibonacci(n - 1) + Fibonacci(n - 2);
    }
}

/// <summary>
/// Provides basic arithmetic operations with comprehensive error handling.
/// </summary>
public class Calculator
{
    /// <summary>
    /// Divides two numbers and handles DivideByZeroException.
    /// </summary>
    /// <param name="a">The numerator.</param>
    /// <param name="b">The denominator.</param>
    /// <returns>The result of the division.</returns>
    /// <exception cref="DivideByZeroException">Thrown if the denominator is zero.</exception>
    public static double Divide(double a, double b)
    {
        if (b == 0)
        {
            throw new DivideByZeroException("Cannot divide by zero.");
        }
        return a / b;
    }
}

/// <summary>
/// Main program class demonstrating methods returning arrays, I/O, and exception handling.
/// </summary>
class Program
{
    /// <summary>
    /// Generates an array of squares for a given size. Demonstrates returning an array.
    /// </summary>
    /// <param name="size">The desired size of the array.</param>
    /// <returns>An array containing the squares of numbers from 0 to size-1.</returns>
    /// <exception cref="ArgumentException">Thrown if size is non-positive.</exception>
    public static int[] GetSquaresArray(int size)
    {
        if (size <= 0)
        {
            throw new ArgumentException("Array size must be a positive integer.", nameof(size));
        }

        int[] squares = new int[size];
        for (int i = 0; i < size; i++)
        {
            squares[i] = i * i;
        }
        return squares;
    }

    /// <summary>
    /// Attempts to read and write to a file, demonstrating file processing with exception handling.
    /// </summary>
    public static void ProcessFile()
    {
        Console.Write("\nEnter file path to process (e.g., test_data.txt): ");
        string filePath = Console.ReadLine();
        
        if (string.IsNullOrWhiteSpace(filePath))
        {
            Console.WriteLine("File path cannot be empty. Skipping file operation.");
            return;
        }

        Console.WriteLine($"\n--- Attempting File Processing for: {filePath} ---");
        StreamReader reader = null; 

        try
        {
            // Write to the file
            File.WriteAllText(filePath, "This is a test line written interactively.\nFile I/O successful.");
            Console.WriteLine("Successfully wrote sample data to the file.");

            // Read the file 
            reader = new StreamReader(filePath);
            string content = reader.ReadToEnd();
            Console.WriteLine("\n--- File Content Read ---");
            Console.WriteLine(content);
        }
        catch (FileNotFoundException e)
        {
            Console.WriteLine($"Error: The file was not found. Details: {e.Message}");
        }
        catch (IOException e)
        {
            Console.WriteLine($"I/O Error: An issue occurred during file access. Details: {e.Message}");
        }
        catch (Exception e)
        {
            Console.WriteLine($"An unexpected error occurred during file processing: {e.Message}");
        }
        finally
        {
            // Ensure the resource is released
            if (reader != null)
            {
                reader.Close(); 
                reader.Dispose();
                Console.WriteLine("File reader resource released in finally block.");
            }
        }
    }
    
    // Helper method to safely read and parse an integer
    static bool TryReadInt(string prompt, out int result)
    {
        Console.Write(prompt);
        string input = Console.ReadLine();
        if (int.TryParse(input, out result))
        {
            return true;
        }
        Console.WriteLine("Invalid input. Please enter a valid integer.");
        return false;
    }

    // Helper method to safely read and parse a double
    static bool TryReadDouble(string prompt, out double result)
    {
        Console.Write(prompt);
        string input = Console.ReadLine();
        if (double.TryParse(input, out result))
        {
            return true;
        }
        Console.WriteLine("Invalid input. Please enter a valid number.");
        return false;
    }


    static void Main(string[] args)
    {
        Console.WriteLine("==================================================");
        Console.WriteLine("          C# Interactive Exception Tester         ");
        Console.WriteLine("==================================================");

        // --- 1. Recursive Mathematical Functions ---
        Console.WriteLine("\n## 📐 Factorial and Fibonacci Tester");
        if (TryReadInt("Enter a non-negative integer N for Factorial and Fibonacci: ", out int n))
        {
            try
            {
                long fact = MathFunctions.Factorial(n);
                Console.WriteLine($"Result: Factorial of {n} is: {fact}");

                int fib = MathFunctions.Fibonacci(n);
                Console.WriteLine($"Result: Fibonacci number at position {n} is: {fib}");
            }
            catch (ArgumentOutOfRangeException e)
            {
                Console.WriteLine($"\n**Math Error (Caught):** {e.Message}");
            }
            catch (Exception e)
            {
                 Console.WriteLine($"\n**General Error (Caught):** An unexpected error occurred: {e.Message}");
            }
        }
        Console.WriteLine(new string('-', 50));


        // --- 2. Calculator with Error Handling ---
        Console.WriteLine("\n## 🧮 Calculator Division Tester (Handles DivideByZero)");
        if (TryReadDouble("Enter Numerator (a): ", out double numA) && 
            TryReadDouble("Enter Denominator (b): ", out double numB))
        {
            try
            {
                double result = Calculator.Divide(numA, numB);
                Console.WriteLine($"Result: {numA} / {numB} = {result}");
            }
            catch (DivideByZeroException e)
            {
                Console.WriteLine($"\n**Calculation Error (Caught):** {e.Message}");
            }
        }
        Console.WriteLine(new string('-', 50));


        // --- 3. Array Method with Exception Handling ---
        Console.WriteLine("\n## 🖼️ Array Generator Tester (Throws on non-positive size)");
        if (TryReadInt("Enter array size (must be > 0): ", out int size))
        {
            try
            {
                int[] squares = GetSquaresArray(size);
                Console.Write($"Result: Squares Array (Size {size}): ");
                Console.WriteLine(string.Join(", ", squares));
            }
            catch (ArgumentException e)
            {
                Console.WriteLine($"\n**Array Creation Error (Caught):** {e.Message}");
            }
        }
        Console.WriteLine(new string('-', 50));


        // --- 4. File Processing with Exception Handling ---
        Console.WriteLine("\n## 📁 File I/O Tester (Uses try-catch-finally)");
        ProcessFile();
        
        Console.WriteLine(new string('=', 50));
        Console.WriteLine("Interactive Program Execution Finished.");
    }
}
