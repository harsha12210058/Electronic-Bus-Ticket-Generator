# Electronic-Bus-Ticket-Generator
:- The Electronic Bus Ticket Generator is a software application that generates electronic tickets for bus passengers.The system also provides features such as real-time bus tracking, seat selection, and fare calculation. In this project, we aim to develop an Electronic Bus Ticket Generator using C programming language.


#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Define structure for bus details
struct Bus {
    int busID;
    char route[50];
    int seats;
    float fare;
};

// Function to display available buses for a route
void displayBuses() {
    FILE *file;
    struct Bus bus;

    file = fopen("buses.dat", "rb");

    if (file == NULL) {
        printf("Error opening file.\n");
        return;
    }

    printf("Available Buses:\n");
    printf("%-10s%-30s%-10s%-10s\n", "Bus ID", "Route", "Seats", "Fare");
    printf("-----------------------------------------------------\n");

    while (fread(&bus, sizeof(struct Bus), 1, file) == 1) {
        printf("%-10d%-30s%-10d%-10.2f\n", bus.busID, bus.route, bus.seats, bus.fare);
    }

    fclose(file);
}

// Function to add a new bus
void addBus() {
    FILE *file;
    struct Bus bus;

    file = fopen("buses.dat", "ab");

    if (file == NULL) {
        printf("Error opening file.\n");
        return;
    }

    printf("Enter Bus ID: ");
    scanf("%d", &bus.busID);
    printf("Enter Route: ");
    scanf(" %[^\n]s", bus.route);
    printf("Enter Number of Seats: ");
    scanf("%d", &bus.seats);
    printf("Enter Fare: ");
    scanf("%f", &bus.fare);

    fwrite(&bus, sizeof(struct Bus), 1, file);

    printf("Bus added successfully.\n");

    fclose(file);
}

// Function to book a seat
void bookSeat() {
    FILE *file;
    struct Bus bus;
    int busID;
    int numSeats;

    file = fopen("buses.dat", "r+b");

    if (file == NULL) {
        printf("Error opening file.\n");
        return;
    }

    printf("Enter Bus ID: ");
    scanf("%d", &busID);

    while (fread(&bus, sizeof(struct Bus), 1, file) == 1) {
        if (bus.busID == busID) {
            printf("Enter Number of Seats to Book: ");
            scanf("%d", &numSeats);

            if (bus.seats >= numSeats) {
                bus.seats -= numSeats;
                fseek(file, -sizeof(struct Bus), SEEK_CUR);
                fwrite(&bus, sizeof(struct Bus), 1, file);
                printf("%d seat(s) booked successfully.\n", numSeats);
            } else {
                printf("Seats not available.\n");
            }

            fclose(file);
            return;
        }
    }

    printf("Bus ID not found.\n");

    fclose(file);
}

// Function to cancel a seat
void cancelSeat() {
    FILE *file;
    struct Bus bus;
    int busID;
    int numSeats;

    file = fopen("buses.dat", "r+b");

    if (file == NULL) {
        printf("Error opening file.\n");
        return;
    }

    printf("Enter Bus ID: ");
    scanf("%d", &busID);

    while (fread(&bus, sizeof(struct Bus), 1, file) == 1) {
        if (bus.busID == busID) {
            printf("Enter Number of Seats to Cancel: ");
            scanf("%d", &numSeats);
                    if ((bus.seats + numSeats) <= 50) {
            bus.seats += numSeats;
            fseek(file, -sizeof(struct Bus), SEEK_CUR);
            fwrite(&bus, sizeof(struct Bus), 1, file);
            printf("%d seat(s) cancelled successfully.\n", numSeats);
        } else {
            printf("Invalid number of seats to cancel.\n");
        }

        fclose(file);
        return;
    }
}

printf("Bus ID not found.\n");

fclose(file);
}

// Function to delete/update a bus
void deleteUpdateBus() {
FILE *file;
struct Bus bus;
int busID;
int choice;
file = fopen("buses.dat", "r+b");

if (file == NULL) {
    printf("Error opening file.\n");
    return;
}

printf("Enter Bus ID: ");
scanf("%d", &busID);

while (fread(&bus, sizeof(struct Bus), 1, file) == 1) {
    if (bus.busID == busID) {
        printf("Bus Details:\n");
        printf("%-10s%-30s%-10s%-10s\n", "Bus ID", "Route", "Seats", "Fare");
        printf("-----------------------------------------------------\n");
        printf("%-10d%-30s%-10d%-10.2f\n", bus.busID, bus.route, bus.seats, bus.fare);

        printf("\n1. Delete Bus\n");
        printf("2. Update Bus Details\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                fseek(file, -sizeof(struct Bus), SEEK_CUR);
                fwrite("\0", sizeof(struct Bus), 1, file);
                printf("Bus deleted successfully.\n");
                break;
            case 2:
                printf("Enter New Route: ");
                scanf(" %[^\n]s", bus.route);
                printf("Enter New Number of Seats: ");
                scanf("%d", &bus.seats);
                printf("Enter New Fare: ");
                scanf("%f", &bus.fare);
                fseek(file, -sizeof(struct Bus), SEEK_CUR);
                fwrite(&bus, sizeof(struct Bus), 1, file);
                printf("Bus details updated successfully.\n");
                break;
            default:
                printf("Invalid choice.\n");
                break;
        }

        fclose(file);
        return;
    }
}

printf("Bus ID not found.\n");

fclose(file);
}

int main() {
int choice;
while (1) {
    printf("\nElectronic Bus Ticket Generator\n");
    printf("1. Display Available Buses\n");
    printf("2. Add New Bus\n");
    printf("3. Book Seat\n");
    printf("4. Cancel Seat\n");
    printf("5. Delete/Update Bus Details\n");
    printf("6. Exit\n");
    printf("Enter your choice: ");
    scanf("%d", &choice);

    switch (choice) {
        case 1:
            displayBuses();
            break;
        case 2:
            addBus();
            break;
        case 3:
            bookSeat();
            break;
        case 4:
            cancelSeat();
            break;
        case 5:
            deleteUpdateBus();
            break;
        case 6:
            printf("Thank you for using the Electronic Bus Ticket Generator.\n");
            exit(0);
        default:
            printf("Invalid choice. Please try again.\n");
            break;
    }
}

return 0;
} 
