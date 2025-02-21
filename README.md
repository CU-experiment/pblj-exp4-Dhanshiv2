Question 1. Write a Java program to implement an ArrayList that stores employee details (ID, Name, and Salary). Allow users to add, update, remove, and search employees.
CODE: import java.util.ArrayList;
import java.util.Scanner;

class Employee {
    int id;
    String name;
    double salary;

    public Employee(int id, String name, double salary) {
        this.id = id;
        this.name = name;
        this.salary = salary;
    }
    public String toString() {
        return "ID: " + id + ", Name: " + name + ", Salary: " + salary;
    }
}
public class EmployeeManagement {
    private static ArrayList<Employee> employees = new ArrayList<>();
    private static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        while (true) {
            System.out.println("\nEmployee Management System");
            System.out.println("1. Add Employee");
            System.out.println("2. Update Employee");
            System.out.println("3. Remove Employee");
            System.out.println("4. Search Employee");
            System.out.println("5. Display All Employees");
            System.out.println("6. Exit");
            System.out.print("Choose an option: ");
            
            int choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline
            
            switch (choice) {
                case 1:
                    addEmployee();
                    break;
                case 2:
                    updateEmployee();
                    break;
                case 3:
                    removeEmployee();
                    break;
                case 4:
                    searchEmployee();
                    break;
                case 5:
                    displayEmployees();
                    break;
                case 6:
                    System.out.println("Exiting...");
                    return;
                default:
                    System.out.println("Invalid choice. Try again.");
            }
        }
    }

    private static void addEmployee() {
        System.out.print("Enter Employee ID: ");
        int id = scanner.nextInt();
        scanner.nextLine();
        System.out.print("Enter Employee Name: ");
        String name = scanner.nextLine();
        System.out.print("Enter Employee Salary: ");
        double salary = scanner.nextDouble();
        scanner.nextLine();
        
        employees.add(new Employee(id, name, salary));
        System.out.println("Employee added successfully.");
    }

    private static void updateEmployee() {
        System.out.print("Enter Employee ID to update: ");
        int id = scanner.nextInt();
        scanner.nextLine();
        
        for (Employee emp : employees) {
            if (emp.id == id) {
                System.out.print("Enter New Name: ");
                emp.name = scanner.nextLine();
                System.out.print("Enter New Salary: ");
                emp.salary = scanner.nextDouble();
                scanner.nextLine();
                System.out.println("Employee details updated.");
                return;
            }
        }
        System.out.println("Employee not found.");
    }

    private static void removeEmployee() {
        System.out.print("Enter Employee ID to remove: ");
        int id = scanner.nextInt();
        scanner.nextLine();
        
        for (Employee emp : employees) {
            if (emp.id == id) {
                employees.remove(emp);
                System.out.println("Employee removed successfully.");
                return;
            }
        }
        System.out.println("Employee not found.");
    }
    private static void searchEmployee() {
        System.out.print("Enter Employee ID to search: ");
        int id = scanner.nextInt();
        scanner.nextLine();
        
        for (Employee emp : employees) {
            if (emp.id == id) {
                System.out.println("Employee found: " + emp);
                return;
            }
        }
        System.out.println("Employee not found.");
    }

    private static void displayEmployees() {
        if (employees.isEmpty()) {
            System.out.println("No employees found.");
            return;
        }
        System.out.println("\nEmployee List:");
        for (Employee emp : employees) {
            System.out.println(emp);
        }
    }
}

QUESTION 2. Create a program to collect and store all the cards to assist the users in finding all the cards in a given symbol using Collection interface.
CODE: import java.util.ArrayList;
import java.util.Collection;
import java.util.Scanner;

class Card {
    private String symbol;
    private String name;

    public Card(String symbol, String name) {
        this.symbol = symbol;
        this.name = name;
    }

    public String getSymbol() {
        return symbol;
    }

    @Override
    public String toString() {
        return "Card{Name='" + name + "', Symbol='" + symbol + "'}";
    }
}

public class CardCollection {
    private static Collection<Card> cards = new ArrayList<>();
    private static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        while (true) {
            System.out.println("\nCard Collection System");
            System.out.println("1. Add Card");
            System.out.println("2. Search Cards by Symbol");
            System.out.println("3. Display All Cards");
            System.out.println("4. Exit");
            System.out.print("Choose an option: ");
            int choice = scanner.nextInt();
            scanner.nextLine(); 

            switch (choice) {
                case 1:
                    addCard();
                    break;
                case 2:
                    searchCardsBySymbol();
                    break;
                case 3:
                    displayCards();
                    break;
                case 4:
                    System.out.println("Exiting...");
                    return;
                default:
                    System.out.println("Invalid choice. Try again.");
            }
        }
    }

    private static void addCard() {
        System.out.print("Enter Card Symbol: ");
        String symbol = scanner.nextLine();
        System.out.print("Enter Card Name: ");
        String name = scanner.nextLine();
        cards.add(new Card(symbol, name));
        System.out.println("Card added successfully.");
    }

    private static void searchCardsBySymbol() {
        System.out.print("Enter Symbol to search: ");
        String symbol = scanner.nextLine();
        boolean found = false;

        for (Card card : cards) {
            if (card.getSymbol().equalsIgnoreCase(symbol)) {
                System.out.println(card);
                found = true;
            }
        }
        if (!found) {
            System.out.println("No cards found with symbol: " + symbol);
        }
    }
    private static void displayCards() {
        if (cards.isEmpty()) {
            System.out.println("No cards in the collection.");
            return;
        }
        System.out.println("\nCard Collection:");
        for (Card card : cards) {
            System.out.println(card);
        }
    }
}

QUESTION 3. Develop a ticket booking system with synchronized threads to ensure no double booking of seats. Use thread priorities to simulate VIP bookings being processed first
CODE: import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

class TicketBookingSystem {
    private int availableSeats;
    private final Lock lock = new ReentrantLock();

    public TicketBookingSystem(int seats) {
        this.availableSeats = seats;
    }

    public void bookSeat(String customer) {
        lock.lock();
        try {
            if (availableSeats > 0) {
                System.out.println(customer + " successfully booked a seat.");
                availableSeats--;
            } else {
                System.out.println("No seats available for " + customer);
            }
        } finally {
            lock.unlock();
        }
    }
}

class BookingThread extends Thread {
    private TicketBookingSystem system;
    private String customer;

    public BookingThread(TicketBookingSystem system, String customer, int priority) {
        this.system = system;
        this.customer = customer;
        setPriority(priority);
    }
    public void run() {
        system.bookSeat(customer);
    }
}
public class TicketBookingApp {
    public static void main(String[] args) {
        TicketBookingSystem system = new TicketBookingSystem(5);

        Thread vip1 = new BookingThread(system, "VIP Customer 1", Thread.MAX_PRIORITY);
        Thread vip2 = new BookingThread(system, "VIP Customer 2", Thread.MAX_PRIORITY);
        Thread regular1 = new BookingThread(system, "Regular Customer 1", Thread.NORM_PRIORITY);
        Thread regular2 = new BookingThread(system, "Regular Customer 2", Thread.NORM_PRIORITY);
        Thread regular3 = new BookingThread(system, "Regular Customer 3", Thread.NORM_PRIORITY);
        Thread regular4 = new BookingThread(system, "Regular Customer 4", Thread.NORM_PRIORITY);

        vip1.start();
        vip2.start();
        regular1.start();
        regular2.start();
        regular3.start();
        regular4.start();
    }
}

