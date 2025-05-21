# Airline Reservation System

We want to create an Airline Reservation System to manage flight schedules, bookings, and seat availability for domestic and international flights. It should handle concurrent bookings, alert users of full flights, and display dynamic pricing, onboard services, and add-ons. Successful bookings should generate an invoice, while unavailable bookings should show a prompt. Managers should be able to update schedules and generate reports on fully booked flights, peak booking periods, and popular routes. Travelers should be able to search flights, book tickets, and view invoices.

---

## Requirements

* **Add Flight:** The manager can add a flight with all its details to the dataset.
* **Depart Flight:** The manager can depart a flight, removing it from the schedule.
* **Delete Flight:** The manager can delete the flight, removing it from the dataset.
* **Report:** A statistical report consisting of:
  * Departed flights
  * Frequent destinations
  * Frequent departure months
  * Frequent booking periods
* **Schedule:** List of all the flights in the dataset which have not been departed.
* **Search Flight:** To search for a flight with specific details.
* **Book Tickets:** The traveler can book tickets for the flight.
* **Add-ons:** The traveler can get add-ons for the booked tickets.
* **Invoice:** A detailed report of all the tickets booked and add-ons purchased by the traveler.

---

## Classes and Objects

### Seat

The `Seat` class represents a seat in an airplane.

#### Attributes
* `(int) position` – This represents the seat number in the airplane.
* `(String) airplane_name` – This represents the name of the airplane the seat is in.
* `(String) passenger_name` – This represents the name of the passenger to whom the seat is booked.
* `(BooleanProperty) booked` – Indicates if the seat is booked (`true`) or not (`false`).
* `(double) seatPrice` – The price of the seat.
* `(Calendar) booking_time` – The time when the seat was booked.
* `(ToggleButton) ref` – The ToggleButton linked to the seat in the seat booking GUI.

#### Methods
* `(int) book(Traveller traveller)` – Synchronized function that allows only one thread to book the seat; it takes a `Traveller` object as a parameter.
* `(void) updateSeatNo(int position)` – Updates the seat number.
* `(void) updateAirlinesName(String airlines_name)` – Updates the airplane name.

#### Requirements Fulfilled
* Requirement 7: Book Tickets.
* Requirement 9: Generating Invoice which shows seat numbers and add-ons booked.

---

### Airplane

The `Airplane` class represents an airplane in the dataset.

#### Attributes
* `(String) name` – Name of the airplane.
* `(String) type` – Type of airplane (Domestic, International, etc.).
* `(String) origin` – Departure location.
* `(String) destination` – Destination location.
* `(Calendar) arrival` – Timestamp when the airplane is arriving.
* `(Calendar) departure` – Timestamp when the airplane is departing.
* `(DoubleProperty) seatPrice` – Price of each seat.
* `(IntegerProperty) bookedSeats` – Number of booked seats.
* `(Seat[]) seats` – Array of seats; the constructor sets the number of seats.
* `(boolean) departed` – True if the airplane has departed, otherwise false.

#### Methods
* `(int) countBooked()` – Returns the number of booked seats.
* `(String) toString()` – Overrides the `toString()` method to provide a string representation containing Flight Name, From Location, To Location, Arrival Time, Departure Time, and Number of Booked Seats.

#### Requirements Fulfilled
* Requirement 1: Add Flight.
* Requirement 2: Depart Flight.
* Requirement 4: Report on departed flights.
* Requirement 5: Schedule of flights.

---

### Stack<T>

A generic `Stack` class that provides a stack data structure for any type.

#### Attributes
* `(ArrayList<T>) arr` – The array list where objects of type T are stored.
* `(int) top` – Represents the topmost index of the stack.

#### Methods
* `(void) push(T data)` – Pushes data onto the stack, incrementing the top index.
* `(T) pop()` – Removes and returns the top element, decrementing the top index.
* `(void) display()` – Displays all objects in the stack.

#### Requirements Fulfilled
* Requirement 4: Providing a statistical report on all departed flights.

---

### Report

The `Report` class represents the report of departed flights and contains methods to generate statistics.

#### Attributes
* `(static Stack<Airplane>) logs` – Stack of all `Airplane` objects that have departed.
* `(stack ObservableList<Airplane>) departedFlights` – ObservableList of all departed flights (for display in a ListView).

#### Methods
* `(static <T> boolean) isPresent(Stack<T> stack, T element)` – Checks if the element is present in the stack.
* `(static <T> int) countElement(Stack<T> stack, T element)` – Returns the count of how many times an element appears in the stack.
* `(static String) getFrequentDeparturePeriod()` – Returns the most frequent departure month.
* `(static String) getFrequentBookingMonth()` – Returns the month when seats were most frequently booked.
* `(static String) getFrequentBookingYear()` – Returns the year when seats were most frequently booked.
* `(static String) getFrequentBookingDay()` – Returns the day of the week when seats were most frequently booked.
* `(static String) getFrequentDestination()` – Returns the most frequently booked destination.

#### Requirements Fulfilled
* Requirement 4: Providing a statistical report on all departed flights.

---

### Request

The `Request` class implements `Runnable` and represents a booking request thread.

#### Attributes
* `(Traveller) traveller` – The traveler initiating the booking request.
* `(Seat) seatObj` – The seat to be booked.
* `(Thread) thread` – The thread associated with this booking request.
* `(int) success` – Set to `1` if booking is successful, and `0` if unsuccessful.

#### Methods
* `(public void) run()` – Overrides `run()` to call the synchronized `book()` function on the seat, passing the traveler as the parameter, and stores the result in `success`.

#### Requirements Fulfilled
* Requirement 7: Book Tickets.

---

### Schedule

The `Schedule` class represents the schedule of flights that have not yet departed.

#### Attributes
* `(static ObservableList<Airplane>) schedule` – List of `Airplane` objects that have not departed, made ObservableList for display in a ListView.
* `(static int) top` – Represents the topmost index of the schedule list.

#### Requirements Fulfilled
* Requirement 5: Display a schedule of all flights yet to depart.

---

### Traveller

The `Traveller` class represents a traveler and extends from the `Schedule` class.

#### Attributes
* `(ObservableList<String>) seats_booked` – List of strings representing booked seats and their corresponding prices.
* `(ObservableList<String>) addons_booked` – List of strings representing purchased add-ons and their corresponding prices.
* `(private String) passenger_name` – Traveler’s name (private to prevent external modification).
* `(Airplane) airplane` – Reference to the airplane the traveler intends to book.
* `(double) totalCost` – Total amount spent by the traveler, including seats and add-ons.

#### Methods
* `(String) getPassengerName()` – Returns the traveler's name.
* `(int) bookSeats(Seat seatObj)` – Called when a traveler clicks a seat button in the booking interface; creates a new thread to execute the `book()` function on the seat. On success, increases the price of the other unbooked seats by 10% and returns the success (1) or failure (0) value.

#### Requirements Fulfilled
* Requirement 7: Book Tickets.
* Requirement 9: View the invoice containing booked seats and add-ons.

---

### Manager

The `Manager` class represents the system administrator responsible for flight management.

#### Methods
* `(static int) searchEntry(String airplane_name)` – Searches for an entry in the dataset.
* `(static boolean) deleteEntry(String airplane_name)` – Deletes a flight entry.
* `(static boolean) setDeparted(String airplane_name)` – Marks a flight as departed.
* `(static void) addEntry(String name, String type, int seat_capacity, double seatPrice, String origin, String destination, int arrival_minute, int arrival_hour, int arrival_day, int arrival_month, int arrival_year, int departure_minute, int departure_hour, int departure_day, int departure_month, int departure_year)` – Adds a flight to the dataset.

#### Requirements Fulfilled
* Requirement 1: Add Flights.
* Requirement 2: Depart Flights.
* Requirement 3: Delete Flights.
* Requirement 6: Search Flights.

---

### concurrentTraveller

The `concurrentTraveller` class implements `Runnable` and simulates another traveler attempting to book the same flight concurrently.

#### Attributes
* `(private Traveller) traveller` – The concurrent traveler.
* `(Airplane) toBookAirplane` – The airplane the traveler is trying to book.
* `(Thread) travellerConsole` – The thread associated with this concurrent booking.

#### Methods
* `(public void) run()` – Overrides `run()` to set the traveler's airplane to `toBookAirplane` and takes seat inputs from the console to book seats. If all seats are booked, a message is displayed; otherwise, the process continues until all seats are booked or the exit command (`-1`) is entered, simulating seat booking by multiple travelers concurrently.

#### Requirements Fulfilled
* Requirement 7: Book Tickets (demonstrating multithreading during seat booking).

---

### Demo

The `Demo` class is the main public class that extends the JavaFX `Application` class to open the GUI and interact with the user.

#### Attributes
* `(private Traveller) traveller` – Represents the main traveler interacting with the system.

#### Methods
* `(public static void) main(String[] args)` – The main function that launches the JavaFX application.
* `(public void) start(Stage ps)` – Starts the JavaFX application, allowing the user to log in as a Traveler or Manager.
* `(public void) deleteScene(Stage primaryStage, Scene oldScene)` – Sets the scene for the manager dashboard to delete a flight; includes features to check available flights and confirm deletion.
* `(public void) addonScene(Stage primaryStage, Scene Dashboard)` – Switches to the add-ons scene for purchasing add-ons after booking seats.
* `(public void) reportScene(Stage primaryStage, Scene oldScene)` – Displays the report scene where the Manager can view statistics on departed flights.
* `(public void) invoiceScene(Stage travellerStage, Scene oldScene)` – Shows the invoice scene detailing booked seats, add-ons purchased, and total cost.
* `(public void) scheduleScene(Stage arbitraryStage, Scene oldScene)` – Displays the schedule of flights yet to depart and provides options to search through flights.
* `(public void) bookPrompt(Stage primaryStage, Scene oldScene)` – Sets the scene where travelers can view available flights and enter the flight name to book.
* `(public void) bookScene(Stage primaryStage, Scene oldScene, Scene Dashboard)` – Displays the seat selection booking scene for travelers.
* `(public void) userWindow()` – Opens the traveler dashboard, including options for adding flights.
* `(public void) adminWindow()` – Opens the manager dashboard with all necessary management features.

#### Requirements Fulfilled
* Requirement 1: Add Flight.
* Requirement 2: Depart Flight.
* Requirement 3: Delete Flight.
* Requirement 4: Check report of departed flights.
* Requirement 5: Check schedule of available flights.
* Requirement 6: Search through flights.
* Requirement 7: Book Tickets.
* Requirement 8: Purchase add-ons after booking seats.
* Requirement 9: Generate invoice after the purchasing process.
