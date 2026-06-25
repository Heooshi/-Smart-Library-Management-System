import java.util.ArrayList;
import java.util.Scanner;

class Book {
    private int id;
    private String title;
    private String author;
    private boolean available;

    public Book(int id, String title, String author) {
        this.id = id;
        this.title = title;
        this.author = author;
        this.available = true;
    }

    public int getId() {
        return id;
    }

    public String getTitle() {
        return title;
    }

    public boolean isAvailable() {
        return available;
    }

    public void borrowBook() {
        available = false;
    }

    public void returnBook() {
        available = true;
    }

    public void displayBook() {
        System.out.println("ID: " + id +
                " | Title: " + title +
                " | Author: " + author +
                " | Available: " + available);
    }
}

class Member {
    private int memberId;
    private String name;
    private ArrayList<Book> borrowedBooks;

    public Member(int memberId, String name) {
        this.memberId = memberId;
        this.name = name;
        borrowedBooks = new ArrayList<>();
    }

    public int getMemberId() {
        return memberId;
    }

    public void borrow(Book book) {
        borrowedBooks.add(book);
    }

    public void returnBook(Book book) {
        borrowedBooks.remove(book);
    }

    public void displayMember() {
        System.out.println("Member ID: " + memberId +
                " | Name: " + name);
    }
}

class Library {
    private ArrayList<Book> books;
    private ArrayList<Member> members;

    public Library() {
        books = new ArrayList<>();
        members = new ArrayList<>();
    }

    public void addBook(Book book) {
        books.add(book);
        System.out.println("Book added successfully.");
    }

    public void addMember(Member member) {
        members.add(member);
        System.out.println("Member added successfully.");
    }

    public Book findBook(int id) {
        for (Book book : books) {
            if (book.getId() == id) {
                return book;
            }
        }
        return null;
    }

    public Member findMember(int id) {
        for (Member member : members) {
            if (member.getMemberId() == id) {
                return member;
            }
        }
        return null;
    }

    public void displayBooks() {
        for (Book book : books) {
            book.displayBook();
        }
    }
}

public class LibraryManagementSystem {
    public static void main(String[] args) {

        Scanner input = new Scanner(System.in);
        Library library = new Library();

        while (true) {
            System.out.println("\n===== Library Menu =====");
            System.out.println("1. Add Book");
            System.out.println("2. Add Member");
            System.out.println("3. Borrow Book");
            System.out.println("4. Return Book");
            System.out.println("5. Display Books");
            System.out.println("6. Exit");

            int choice = input.nextInt();

            switch (choice) {

                case 1:
                    System.out.print("Book ID: ");
                    int bookId = input.nextInt();
                    input.nextLine();

                    System.out.print("Title: ");
                    String title = input.nextLine();

                    System.out.print("Author: ");
                    String author = input.nextLine();

                    library.addBook(new Book(bookId, title, author));
                    break;

                case 2:
                    System.out.print("Member ID: ");
                    int memberId = input.nextInt();
                    input.nextLine();

                    System.out.print("Member Name: ");
                    String name = input.nextLine();

                    library.addMember(new Member(memberId, name));
                    break;

                case 3:
                    System.out.print("Book ID: ");
                    int borrowBookId = input.nextInt();

                    System.out.print("Member ID: ");
                    int borrowMemberId = input.nextInt();

                    Book book = library.findBook(borrowBookId);
                    Member member = library.findMember(borrowMemberId);

                    if (book != null && member != null && book.isAvailable()) {
                        book.borrowBook();
                        member.borrow(book);
                        System.out.println("Book borrowed successfully.");
                    } else {
                        System.out.println("Book unavailable or invalid data.");
                    }
                    break;

                case 4:
                    System.out.print("Book ID: ");
                    int returnBookId = input.nextInt();

                    System.out.print("Member ID: ");
                    int returnMemberId = input.nextInt();

                    Book returnedBook = library.findBook(returnBookId);
                    Member returnedMember = library.findMember(returnMemberId);

                    if (returnedBook != null && returnedMember != null) {
                        returnedBook.returnBook();
                        returnedMember.returnBook(returnedBook);
                        System.out.println("Book returned successfully.");
                    } else {
                        System.out.println("Invalid data.");
                    }
                    break;

                case 5:
                    library.displayBooks();
                    break;

                case 6:
                    System.out.println("Goodbye!");
                    System.exit(0);

                default:
                    System.out.println("Invalid choice.");
            }
        }
    }
}
