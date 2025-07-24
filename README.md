# Digital-Library-Management-System

import java.util.*;

class Book {
    int id;
    String title;
    boolean isIssued;

    Book(int id, String title) {
        this.id = id;
        this.title = title;
        this.isIssued = false;
    }
}

public class Main {
    static Scanner sc = new Scanner(System.in);
    static List<Book> books = new ArrayList<>();
    static Map<String, String> users = new HashMap<>();
    static String adminUser = "admin", adminPass = "admin123";

    public static void main(String[] args) {
        books.add(new Book(1001, "Java Basics"));
        books.add(new Book(1002, "C Programming"));
        books.add(new Book(1003, "Data Structures"));
        users.put("user", "user123");

        System.out.print("Login as (admin/user): ");
        String role = sc.next();

        System.out.print("Enter username: ");
        String u = sc.next();
        System.out.print("Enter password: ");
        String p = sc.next();

        if (role.equals("admin") && u.equals(adminUser) && p.equals(adminPass)) adminPanel();
        else if (role.equals("user") && users.containsKey(u) && users.get(u).equals(p)) userPanel();
        else System.out.println("Invalid login.");
    }

    static void adminPanel() {
        while (true) {
            System.out.println("\n1. View Books\n2. Add Book\n3. Remove Book\n4. Logout");
            int choice = sc.nextInt();
            if (choice == 1) displayBooks();
            else if (choice == 2) {
                System.out.print("Enter Book ID and Title: ");
                int id = sc.nextInt();
                String title = sc.nextLine().trim() + sc.nextLine();
                books.add(new Book(id, title));
                System.out.println("Book added.");
            } else if (choice == 3) {
                System.out.print("Enter Book ID to remove: ");
                int id = sc.nextInt();
                books.removeIf(b -> b.id == id);
                System.out.println("Book removed.");
            } else break;
        }
    }

    static void userPanel() {
        while (true) {
            System.out.println("\n1. View Books\n2. Issue Book\n3. Return Book\n4. Logout");
            int choice = sc.nextInt();
            if (choice == 1) displayBooks();
            else if (choice == 2) {
                System.out.print("Enter Book ID to issue: ");
                int id = sc.nextInt();
                for (Book b : books)
                    if (b.id == id && !b.isIssued) {
                        b.isIssued = true;
                        System.out.println("Book issued.");
                        break;
                    } else if (b.id == id) {
                        System.out.println("Book already issued.");
                        break;
                    }
            } else if (choice == 3) {
                System.out.print("Enter Book ID to return: ");
                int id = sc.nextInt();
                for (Book b : books)
                    if (b.id == id && b.isIssued) {
                        b.isIssued = false;
                        System.out.println("Book returned.");
                        break;
                    } else if (b.id == id) {
                        System.out.println("Book was not issued.");
                        break;
                    }
            } else break;
        }
    }

    static void displayBooks() {
        for (Book b : books)
            System.out.println(b.id + ". " + b.title + (b.isIssued ? " [Issued]" : " [Available]"));
    }
}
