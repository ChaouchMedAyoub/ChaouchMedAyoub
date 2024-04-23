#include <stdio.h>
#include <string.h>
#include <stdlib.h>


struct Date {
    int day;
    int month;
    int year;
};


struct Client {
    int Id;
    char First_Name[50];
    char Name[50];
    struct Date Date_of_Birth;
    char Address[100];
    char Tel[20];
};


struct Account {
    int ID;
    char Account_Type[20];
    int Balance;
    int Blocked;
};

struct Account2 {
    int ID2;
    char Account_Type2[20];
    int Balance2;
    int Blocked2;
};


void ask_and_print(struct Client *a);

void deposit(struct Account *a);
void information_of_account_b(struct Account2 *b);
void read_account_data(struct Account *q, const char *filename) ;
void withdrawal(struct Account *a);
void modify_account(struct Account *a);
void account_inquiry(struct Account *a);


int main() {
    struct Client a;
    struct Account q;
    struct Account2 b;
    int amount;
    int operation;

  
    char s[20] = "Individual";
    char w[20] = "Minor";
    char f[20] = "Commercial";
    int menu;
  

     ask_and_print(&a);

    printf("Choose your account TYPE\n");
    printf("1. Individual\n");
    printf("2. Minor\n");
    printf("3. Commercial\n");
    printf(">> ");
    scanf("%d", &menu);

    if (menu == 1 || menu == 2 || menu == 3) {
        if (menu == 1) {
            if (2024 - a.Date_of_Birth.year >= 18) {
                printf("Your account has been created successfully\n");
                strcpy(q.Account_Type, s);
                q.ID = a.Id;
                q.Balance = 0;
            } else {
                printf("ERROR! You must be at least 18 years old\n");
            }
        } else if (menu == 2 && 2024 - a.Date_of_Birth.year <= 18) {
            int Id1;
            printf("Enter your guardian's account Id: ");
            scanf("%d", &Id1);
            if (a.Id == Id1) {
                printf("Your account has been created successfully\n");
                strcpy(q.Account_Type, w);
                q.ID = a.Id;
                q.Balance = 0;
            } else {
                printf("ERROR! Guardian ID does not match or you are over 18 years old\n");
            }
        } else if (menu == 3) {
            printf("Your account has been created successfully\n");
            strcpy(q.Account_Type, f);
            q.ID = a.Id;
            q.Balance = 0;
        }
    } else {
        printf("ERROR! Invalid menu option\n");
    }

    do {
        printf("\nSelect the desired operation:\n");
        printf("1. Deposit (T)\n");
        printf("2. Transfer (V)\n");
        printf("3. Withdrawal (R)\n");
        printf("4. Modification (M)\n");
        printf("5. Account Inquiry (C)\n");
        printf("6. Exit\n");
        printf(">> ");
        scanf("%d", &operation);

        switch (operation) {
            case 1:
                deposit(&q);
                break;
            case 2:
    printf ("The second acount -> \n");
               break;
            case 3:
                withdrawal(&q);
                break;
            case 4:
                modify_account(&q);
                break;
            case 5:
                account_inquiry(&q);
                break;
            case 6:
                printf("Exiting program.\n");
                break;
            default:
                printf("Invalid operation. Please try again.\n");
        }
 if (operation == 2){
            struct Account2 b;


    printf ("ID of the second Acccount :");
       scanf("%d" , &b.ID2);

    printf ("TYPE of the second Acccount :");
       scanf("%s" , &b.Account_Type2);
    printf (" Balance of the second Acccount :");
       scanf("%d" , &b.Balance2);
    printf (" second Acccount is bloked  ??  (Yes = 1 or No = 0) :");
      scanf("%d" , &b.Blocked2); 
        
        printf("Enter the amount to transfer: ");
    scanf("%d", &amount);

    if (amount > 0 && q.Balance >= amount) {
        q.Balance -= amount;
        b.Balance2 += amount;
        printf("Successfully transferred %d from account ID %d to account ID %d.\n", amount, q.ID, b.ID2);
    } else {
        printf("Invalid amount or insufficient funds. Please enter a positive number and ensure you have enough balance.\n");
    }

    printf("Successfully transferred.\n");
    }

    } while (operation != 6);

    return 0;
}


void ask_and_print(struct Client *a) {
   FILE *file = fopen("client_info.txt", "w");
    if (file == NULL) {
        printf("Error opening file!\n");
        exit(1);
    }

    printf("Enter your Id: ");
    scanf("%d", &a->Id);

    printf("Enter your First Name: ");
    scanf("%s", a->First_Name);

    printf("Enter your name: ");
    scanf("%s", a->Name);

    printf("Enter your date of birth (dd mm yyyy): ");
    scanf("%d %d %d", &a->Date_of_Birth.day, &a->Date_of_Birth.month, &a->Date_of_Birth.year);

    printf("Enter your Address: ");
    scanf("%s", a->Address);

    printf("Enter your Tel: ");
    scanf("%s", a->Tel);

    
    fprintf(file, "Id: %d\n", a->Id);
    fprintf(file, "First Name: %s\n", a->First_Name);
    fprintf(file, "Name: %s\n", a->Name);
    fprintf(file, "Date of Birth: %02d/%02d/%04d\n", a->Date_of_Birth.day, a->Date_of_Birth.month, a->Date_of_Birth.year);
    fprintf(file, "Address: %s\n", a->Address);
    fprintf(file, "Tel: %s\n", a->Tel);
}
void read_account_data(struct Account *q, const char *filename) {
    FILE *file = fopen(filename, "r");
    if (file == NULL) {
        printf("Error opening file!\n");
        exit(1);
    }
    
    // Read  account data from the file
    fscanf(file, "%d %s %d %d", &q->ID, q->Account_Type, &q->Balance, &q->Blocked);
    
    // Close  file
    fclose(file);
}

// Function to deposit money into  account and update  file
void deposit(struct Account *q) {
    int mony;
    printf("Enter the amount to deposit: ");
    scanf("%d", &mony);

    if (mony > 0) {
        q->Balance += mony;
        printf("Successfully deposited to your account ID %d.\n", q->ID);
        
        // Open the file in write mode to update the account balance
        FILE *file = fopen("account_data.txt", "w");
        if (file == NULL) {
            printf("Error opening file!\n");
            return;
        }
        
        // Write the updated account data to the file
        fprintf(file, "ID of the account : %d\n Account Type: %s \n Balance: %d\n Blocked : %d\n", q->ID, q->Account_Type, q->Balance, q->Blocked);
        
        // Close the file
        fclose(file);
    } else {
        printf("Invalid amount. Please enter a positive number.\n");
    }
}



void information_of_account_b(struct Account2 *b){

    int ID2;
    char* Account_Type2[20];
    int Balance2;
    int Blocked2;  

    printf ("ID of the second Acccount :");
       scanf("%d" , &b->ID2);

    printf ("TYPE of the second Acccount :");
       scanf("%s" , &b->Account_Type2);
    printf (" Balance of the second Acccount :");
       scanf("%d" , &b->Balance2);
    printf (" second Acccount is bloked  ??  (Yes = 1 or No = 0) :");
      scanf("%d" , &b->Blocked2);
}




void withdrawal(struct Account *q) {
    int withdrawalAmount;
    printf("Enter the amount to withdraw: ");
    scanf("%d", &withdrawalAmount);

    if (withdrawalAmount <= 0) {
        printf("Please enter a positive number.\n");
        return;
    }

    if (q->Balance >= withdrawalAmount) {
        q->Balance -= withdrawalAmount;
        printf("Successfully withdrew %d from account ID %d.\n", withdrawalAmount, q->ID);
        
        // Update the account information in the file
        FILE *file = fopen("account_data.txt", "w");
        if (file == NULL) {
            printf("Error opening file!\n");
            return;
        }
        
        // Write the updated account data to the file
        fprintf(file, "ID of the account : %d\n Account Type: %s \n Balance: %d\n Blocked : %d\n", q->ID, q->Account_Type, q->Balance, q->Blocked);
        
        // Close the file
        fclose(file);
    } else {
        printf("Insufficient funds for withdrawal.\n");
    }
}
void modify_account(struct Account *q) {
    int menu;
    printf("Choose the new account type:\n");
    printf("1. Savings\n");
    printf("2. Checking\n");
    printf("3. Business\n");
    printf(">> ");
    scanf("%d", &menu);

    switch (menu) {
        case 1:
            strcpy(q->Account_Type, "Savings");
            printf("Account type changed to Savings.\n");
            break;
        case 2:
            strcpy(q->Account_Type, "Checking");
            printf("Account type changed to Checking.\n");
            break;
        case 3:
            strcpy(q->Account_Type, "Business");
            printf("Account type changed to Business.\n");
            break;
        default:
            printf("Invalid choice. No changes made.\n");
    }
}
void account_inquiry(struct Account *q) {

    printf("Account ID: %d\n", q->ID);
    printf("Account Type: %s\n", q->Account_Type);
    printf("Balance: %d\n", q->Balance);
    printf("Blocked: %s\n", (q->Blocked == 1) ? "Yes" : "No");
}
