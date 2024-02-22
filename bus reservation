#include <stdio.h>
#include <stdlib.h>

struct Reservation {
    char passengerName[50];
    char gender; // 'M' for male, 'F' for female, or other codes as needed
    int numberOfPassengers;
    char departureLocation[50];
    char arrivalLocation[50];
    char travelDate[20];
    int isCancelled; // 1 if cancelled, 0 if not cancelled
    float paymentAmount;
};

void makeReservation();
void displayReservation();
void cancelReservation();

int main() {
    int choice;

    do {
        printf("\nBus Reservation System\n");
        printf("1. Make Reservation\n");
        printf("2. Display Reservation\n");
        printf("3. Cancel Reservation\n");
        printf("4. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                makeReservation();
                break;
            case 2:
                displayReservation();
                break;
            case 3:
                cancelReservation();
                break;
            case 4:
                printf("Exiting the program.\n");
                break;
            default:
                printf("Invalid choice. Please enter a valid option.\n");
        }
    } while (choice != 4);

    return 0;
}

void makeReservation() {
    FILE *filePointer;
    struct Reservation reservation;

    // Open a file in append mode
    filePointer = fopen("reservations.txt", "a");

    if (filePointer == NULL) {
        printf("Unable to open the file for reservation.\n");
        return;
    }

    printf("Enter Passenger Name: ");
    scanf("%s", reservation.passengerName);
    printf("Enter Gender (M/F): ");
    scanf(" %c", &reservation.gender); // Note the space before %c to consume the newline character
    printf("Enter Number of Passengers: ");
    scanf("%d", &reservation.numberOfPassengers);
    printf("Enter Departure Location: ");
    scanf("%s", reservation.departureLocation);
    printf("Enter Arrival Location: ");
    scanf("%s", reservation.arrivalLocation);
    printf("Enter Travel Date: ");
    scanf("%s", reservation.travelDate);
    reservation.isCancelled = 0; // Initialize to not cancelled
    reservation.paymentAmount = 0.0; // Initialize payment to 0

    // Write reservation details to the file
    fprintf(filePointer, "%s %c %d %s %s %s %d %.2f\n",
            reservation.passengerName, reservation.gender,
            reservation.numberOfPassengers, reservation.departureLocation,
            reservation.arrivalLocation, reservation.travelDate,
            reservation.isCancelled, reservation.paymentAmount);

    // Close the file
    fclose(filePointer);

    printf("Reservation successful!\n");
}

void displayReservation() {
    FILE *filePointer;
    struct Reservation reservation;

    // Open the file in read mode
    filePointer = fopen("reservations.txt", "r");

    if (filePointer == NULL) {
        printf("No reservations found.\n");
        return;
    }

    printf("\nReservation Details:\n");

    // Read and display reservation details from the file
    while (fscanf(filePointer, "%s %c %d %s %s %s %d %f\n",
                  reservation.passengerName, &reservation.gender,
                  &reservation.numberOfPassengers, reservation.departureLocation,
                  reservation.arrivalLocation, reservation.travelDate,
                  &reservation.isCancelled, &reservation.paymentAmount) != EOF) {
        printf("Passenger Name: %s\n", reservation.passengerName);
        printf("Gender: %c\n", reservation.gender);
        printf("Number of Passengers: %d\n", reservation.numberOfPassengers);
        printf("Departure Location: %s\n", reservation.departureLocation);
        printf("Arrival Location: %s\n", reservation.arrivalLocation);
        printf("Travel Date: %s\n", reservation.travelDate);
        printf("Cancelled: %s\n", (reservation.isCancelled ? "Yes" : "No"));
        printf("Payment Amount: $%.2f\n", reservation.paymentAmount);
        printf("-------------------------\n");
    }

    // Close the file
    fclose(filePointer);
}

void cancelReservation() {
    FILE *filePointer;
    char targetName[50];
    struct Reservation reservation;

    // Open the file in read-write mode
    filePointer = fopen("reservations.txt", "r+");

    if (filePointer == NULL) {
        printf("No reservations found.\n");
        return;
    }

    printf("Enter Passenger Name for cancellation: ");
    scanf("%s", targetName);

    // Search for the reservation
    int found = 0;
    while (fscanf(filePointer, "%s %c %d %s %s %s %d %f\n",
                  reservation.passengerName, &reservation.gender,
                  &reservation.numberOfPassengers, reservation.departureLocation,
                  reservation.arrivalLocation, reservation.travelDate,
                  &reservation.isCancelled, &reservation.paymentAmount) != EOF) {
        if (strcmp(reservation.passengerName, targetName) == 0) {
            found = 1;
            if (reservation.isCancelled) {
                printf("Reservation is already cancelled.\n");
            } else {
                reservation.isCancelled = 1;
                printf("Reservation for %s cancelled successfully.\n", targetName);
                // Move the file pointer back to the beginning of the record
                fseek(filePointer, -sizeof(struct Reservation), SEEK_CUR);
                // Write the updated reservation details to the file
                fprintf(filePointer, "%s %c %d %s %s %s %d %.2f\n",
                        reservation.passengerName, reservation.gender,
                        reservation.numberOfPassengers, reservation.departureLocation,
                        reservation.arrivalLocation, reservation.travelDate,
                        reservation.isCancelled, reservation.paymentAmount);
            }
            break;
        }
    }

    if (!found) {
        printf("Reservation not found.\n");
    }

    // Close the file
    fclose(filePointer);
}
