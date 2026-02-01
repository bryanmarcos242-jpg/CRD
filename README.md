import java.io.*;
import java.util.ArrayList;
import java.util.Scanner;

public class RideBookingApp implements Serializable {
    static class RideBooking implements Serializable {
        int bookingId;
        String passengerName;
        String pickup;
        String destination;

        RideBooking(int bookingId, String passengerName, String pickup, String destination) {
            this.bookingId = bookingId;
            this.passengerName = passengerName;
            this.pickup = pickup;
            this.destination = destination;
        }

        @Override
        public String toString() {
            return "Booking ID: " + bookingId +
                   " | Passenger: " + passengerName +
                   " | Pickup: " + pickup +
                   " | Destination: " + destination;
        }
    }
    static Scanner sc = new Scanner(System.in);
    static ArrayList<RideBooking> bookings = loadBookings();
    public static void main(String[] args) {
        int choice;
        do {
            System.out.println("\n--- RIDE BOOKING SYSTEM ---");
            System.out.println("1. Book Ride");
            System.out.println("2. View Bookings");
            System.out.println("3. Update Booking");
            System.out.println("4. Cancel Booking");
            System.out.println("5. Exit");
            System.out.print("Choose: ");
            choice = sc.nextInt();
            sc.nextLine(); // clear buffer

            switch (choice) {
                case 1 -> createBooking();
                case 2 -> viewBookings();
                case 3 -> updateBooking();
                case 4 -> deleteBooking();
                case 5 -> {
                    saveBookings();
                    System.out.println("Exiting... Thank you for using the Ride Booking System!");
                }
                default -> {
                    System.out.println("Invalid choice!");
                    pause();
                }
            }
        } while (choice != 5);
    }
    static void createBooking() {
        System.out.print("Booking ID: ");
        int id = sc.nextInt();
        sc.nextLine(); // clear buffer

        System.out.print("Passenger Name: ");
        String name = sc.nextLine();

        System.out.print("Pickup Location: ");
        String pickup = sc.nextLine();

        System.out.print("Destination: ");
        String destination = sc.nextLine();

        bookings.add(new RideBooking(id, name, pickup, destination));
        saveBookings();
        System.out.println("Ride booked successfully!");
        pause();
    }
    static void viewBookings() {
        if (bookings.isEmpty()) {
            System.out.println("No bookings found.");
            pause();
            return;
        }
        System.out.println("\n--- BOOKINGS ---");
        for (RideBooking r : bookings) {
            System.out.println(r);
        }
        pause();
    }
    static void updateBooking() {
        System.out.print("Enter Booking ID to update: ");
        int id = sc.nextInt();
        sc.nextLine(); // clear buffer

        for (RideBooking r : bookings) {
            if (r.bookingId == id) {
                System.out.print("New Passenger Name: ");
                r.passengerName = sc.nextLine();

                System.out.print("New Pickup Location: ");
                r.pickup = sc.nextLine();

                System.out.print("New Destination: ");
                r.destination = sc.nextLine();

                saveBookings();
                System.out.println("Booking updated!");
                pause();
                return;
            }
        }
        System.out.println("Booking not found.");
        pause();
    }
    static void deleteBooking() {
        System.out.print("Enter Booking ID to cancel: ");
        int id = sc.nextInt();
        sc.nextLine(); // clear buffer

        boolean removed = bookings.removeIf(r -> r.bookingId == id);
        if (removed) {
            saveBookings();
            System.out.println("Booking cancelled.");
        } else {
            System.out.println("Booking not found.");
        }
        pause();
    }
    static void saveBookings() {
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("ridebookings.dat"))) {
            oos.writeObject(bookings);
        } catch (IOException e) {
            System.out.println("Error saving file.");
        }
    }
    static ArrayList<RideBooking> loadBookings() {
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("ridebookings.dat"))) {
            return (ArrayList<RideBooking>) ois.readObject();
        } catch (Exception e) {
            return new ArrayList<>();
        }
    }
    static void pause() {
        System.out.println("\nPress Enter to continue...");
        sc.nextLine();
    }
}
