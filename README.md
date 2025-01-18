# MultiCast-Delegate
using System;
using System.IO;

public delegate void PrintDelegate();
public delegate void MyDelegate();
public delegate void MathOperationHandler(int a, int b);

class Calculator
{
    public event MathOperationHandler AddEvent;
    public event MathOperationHandler SubtractEvent;

    public void Add(int a, int b)
    {
        AddEvent?.Invoke(a, b);
    }

    public void Subtract(int a, int b)
    {
        SubtractEvent?.Invoke(a, b);
    }
}

class Program
{
    static void Main()
    {
        // Delegate to call methods
        PrintDelegate printDelegate = PrintToConsole;
        printDelegate += PrintToFile;

        printDelegate();

        // Combining delegates
        MyDelegate firstDelegate = FirstMethod;
        MyDelegate secondDelegate = SecondMethod;
        
        MyDelegate multiDelegate = firstDelegate + secondDelegate;

        Console.WriteLine("First Delegate:");
        firstDelegate();

        Console.WriteLine("Second Delegate:");
        secondDelegate();

        Console.WriteLine("MultiDelegate:");
        multiDelegate();

        // Events for addition and subtraction
        Calculator calc = new Calculator();
        calc.AddEvent += OnAdd;
        calc.SubtractEvent += OnSubtract;

        calc.Add(5, 3);
        calc.Subtract(5, 3);
    }

    static void PrintToConsole()
    {
        Console.WriteLine("Hello from Console");
    }

    static void PrintToFile()
    {
        string path = "output.txt";
        File.WriteAllText(path, "Hello from File");
        Console.WriteLine("Message written to file.");
    }

    static void FirstMethod()
    {
        Console.WriteLine("Hello");
    }

    static void SecondMethod()
    {
        Console.WriteLine("Welcome");
    }

    static void OnAdd(int a, int b)
    {
        Console.WriteLine($"Addition: {a + b}");
    }

    static void OnSubtract(int a, int b)
    {
        Console.WriteLine($"Subtraction: {a - b}");
    }
}
