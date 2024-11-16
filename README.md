# User-managment-C-
This is a C++ project using the concepts of Object Oriented Programming which is a simple online store providing simple sale and purchase of products and storing them within a file .

#include <iostream>
#include <fstream>
#include <sstream>
using namespace std;

const int MAX_ITEMS = 300;

// USER MANAGEMENT

class User {
private:
    string username;
    string password;

public:
    User() {
        cout << "WELCOME TO USER" << endl;
    }

    void registerName() {
        cout << "Enter name: ";
        cin >> username;

        cout << "Enter password: ";
        cin >> password;

        ofstream file("users.txt", ios::app);
        if (file.is_open()) {
            file << username << "," << password << endl;
            file.close();
            cout << "Successfully registered" << endl;
        } else {
            cout << "Unable to open file for writing." << endl;
        }
    }

    bool loginUser() {
        string inputUsername, inputPassword;
        cout << "Enter existing username: ";
        cin >> inputUsername;

        cout << "Enter password: ";
        cin >> inputPassword;

        ifstream file("users.txt");
        if (file.is_open()) {
            string line;
            bool found = false;

            while (getline(file, line)) {
                stringstream ss(line);
                string storedUsername, storedPassword;

                getline(ss, storedUsername, ',');
                getline(ss, storedPassword);

                storedPassword.erase(0, storedPassword.find_first_not_of(" "));

                if (inputUsername == storedUsername && inputPassword == storedPassword) {
                    cout << "\nSuccessfully logged in!" << endl;
                    found = true;
                    file.close();
                    return true;
                }
            }

            if (!found) {
                cout << "Invalid username or password" << endl;
            }

            file.close();
        } else {
            cout << "Unable to open file for reading." << endl;
        }
        return false;
    }
};

class Order {
private:
    int orderId;
    string username;
    string shippingMethod;
    double orderPrice;
    static int nextOrderId; // Static variable to keep track of the next available order ID

public:
    Order() : orderId(0), orderPrice(0.0) {}

    Order(int id, const string& user, const string& shipping, double price)
        : orderId(id), username(user), shippingMethod(shipping), orderPrice(price) {}

    void displayOrder() const {
        cout << "\nOrder ID: " << orderId << endl;
//        cout << "Username: " << username << endl;
        cout << "Shipping Method: " << shippingMethod << endl;
        cout << "Order Price: $" << orderPrice << endl;
    }

    void setOrderDetails(int id, const string& user, const string& shipping, double price) {
        orderId = id; // Set the order ID
        username = user;
        shippingMethod = shipping;
        orderPrice = price;
    }

    // Function to get the next available order ID
    static int getNextOrderId() {
        return nextOrderId;
    }

    // Function to increment the order ID
    static void incrementOrderId() {
        nextOrderId++;
    }
};

// Initialize the static variable
int Order::nextOrderId = 1;

// STORE MANAGEMENT

class Store { //abstract class
protected:
    double totalAmount;
    ofstream file;

public:
    Store() : totalAmount(0.0) {}

    virtual void displayProducts() = 0;
    virtual void selectProduct() = 0;

  //  void openFile(const string& filename) {
       // file.open(filename, ios::app);
       // if (!file.is_open()) {
          //  cout << "Unable to open file for writing." << endl;
      //  }//
  //  }

    void closeFile() {
        if (file.is_open()) {
            file.close();
        }
    }

    double getTotalAmount() const {
        return totalAmount;
    }
};

class ClothesStore : public Store {
public:
    void displayProducts() override {
        cout << "Welcome to our cloth store " << endl;
        cout << "1. Jeans, $100" << endl;
        cout << "2. T-shirts, $50" << endl;
        cout << "3. Jacket, $200" << endl;
        cout << "4. Shorts, $30" << endl;
        cout << "5. Sweater, $80" << endl;
    }

    void selectProduct() override {
        int choice;
        cout << "What would you like to buy? : ";
        cin >> choice;

        switch (choice) {
            case 1:
                totalAmount += 100;
                file << "Jeans, $100" << endl;
                break;
            case 2:
                totalAmount += 50;
                file << "T-shirts, $50" << endl;
                break;
            case 3:
                totalAmount += 200;
                file << "Jacket, $200" << endl;
                break;
            case 4:
                totalAmount += 30;
                file << "Shorts, $30" << endl;
                break;
            case 5:
                totalAmount += 80;
                file << "Sweater, $80" << endl;
                break;
            default:
                cout << "Invalid choice" << endl;
                break;
        }
    }
};

class ShoeStore : public Store {
public:
    void displayProducts() override {
        cout << "Welcome to shoe store" << endl;
        cout << "1. Sneakers, $150" << endl;
        cout << "2. Joggers, $100" << endl;
        cout << "3. Walking Shoes, $200" << endl;
        cout << "4. Sandals, $80" << endl;
        cout << "5. Slippers, $50" << endl;
    }

    void selectProduct() override {
        int choice;
        cout << "What type of shoes would you like to buy?" << endl;
        cin >> choice;

        switch (choice) {
            case 1:
                totalAmount += 150;
                file << "Sneakers, $150" << endl;
                break;
            case 2:
                totalAmount += 100;
                file << "Joggers, $100" << endl;
                break;
            case 3:
                totalAmount += 200;
                file << "Walking Shoes, $200" << endl;
                break;
            case 4:
                totalAmount += 80;
                file << "Sandals, $80" << endl;
                break;
            case 5:
                totalAmount += 50;
                file << "Slippers, $50" << endl;
                break;
            default:
                cout << "Invalid choice" << endl;
                break;
        }
    }
};

class perfumeStore : public Store {
public:
    void displayProducts() override {
        cout << "Welcome to perfume store" << endl;
         cout << "1. Floral Perfume, $150" << endl;
        cout << "2. Woody Perfume, $100" << endl;
        cout << "3. Citrus Perfume, $200" << endl;
        cout << "4. Oriental Perfume, $80" << endl;
        cout << "5. Body Mist, $50" << endl;
    }

    void selectProduct() override {
        int choice;
        cout << "What type of Perfume would you like to buy?" << endl;
        cin >> choice;

        switch (choice) {
            case 1:
                totalAmount += 150;
                file << "Floral Perfume, $150" << endl;
                break;
            case 2:
                totalAmount += 100;
                file << " Woody Perfume, $100" << endl;
                break;
            case 3:
                totalAmount += 200;
                file << " Citrus Perfume, $200" << endl;
                break;
            case 4:
                totalAmount += 80;
                file << "Oriental Perfume, $80" << endl;
                break;
            case 5:
                totalAmount += 50;
                file << "Body Mist, $50" << endl;
                break;
            default:
                cout << "Invalid choice" << endl;
                break;
        }
    }
};
class glovesStore : public Store {
public:
    void displayProducts() override {
        cout << "1. Leather gloves, $150" << endl;
        cout << "2. cycling gloves, $100" << endl;
        cout << "3. driving gloves, $200" << endl;
        cout << "4. winter gloves, $80" << endl;
        cout << "5. woolen gloves $50" << endl;
    }

    void selectProduct() override {
        int choice;
        cout << "What type of Gloves would you like to buy?" << endl;
        cin >> choice;

        switch (choice) {
            case 1:
                totalAmount += 150;
                file << "Leather gloves, $150" << endl;
                break;
            case 2:
                totalAmount += 100;
                file << " cycling gloves, $100" << endl;
                break;
            case 3:
                totalAmount += 200;
                file << " driving gloves , $200" << endl;
                break;
            case 4:
                totalAmount += 80;
                file << "winter gloves, $80" << endl;
                break;
            case 5:
                totalAmount += 50;
                file << "woolen gloves, $50" << endl;
                break;
            default:
                cout << "Invalid choice" << endl;
                break;
        }
    }
};
class CosmeticsStore : public Store {
public:
    void displayProducts() override {
        cout << "Welcome to cosmetics store" << endl;
        cout << "1. Foundation, $150" << endl;
        cout << "2. Mascara, $100" << endl;
        cout << "3. Eye Liner, $200" << endl;
        cout << "4. Blush, $80" << endl;
        cout << "5. Eye Shadow, $50" << endl;
    }

    void selectProduct() override {
        int choice;
        cout << "What type of cosmetic would you like to buy?" << endl;
        cin >> choice;

        switch (choice) {
            case 1:
                totalAmount += 150;
                file << "Foundation, $150" << endl;
                break;
            case 2:
                totalAmount += 100;
                file << "Mascara, $100" << endl;
                break;
            case 3:
                totalAmount += 200;
                file << "Eye Liner, $200" << endl;
                break;
            case 4:
                totalAmount += 80;
                file << "Blush, $80" << endl;
                break;
            case 5:
                totalAmount += 50;
                file << "Eye Shadow, $50" << endl;
                break;
            default:
                cout << "Invalid choice" << endl;
                break;
        }
    }
};
class ElectronicsStore : public Store {
public:
    void displayProducts() override {
        cout << "Welcome to electronics store" << endl;
        cout << "1. Smartphone, $700" << endl;
        cout << "2. Laptop, $1200" << endl;
        cout << "3. Headphones, $150" << endl;
        cout << "4. TV, $800" << endl;
        cout << "5. Smartwatch, $300" << endl;
    }

    void selectProduct() override {
        int choice;
        cout << "What type of electronics would you like to buy?" << endl;
        cin >> choice;

        switch (choice) {
            case 1:
                totalAmount += 700;
                file << "Smartphone, $700" << endl;
                break;
            case 2:
                totalAmount += 1200;
                file << "Laptop, $1200" << endl;
                break;
            case 3:
                totalAmount += 150;
                file << "Headphones, $150" << endl;
                break;
            case 4:
                totalAmount += 800;
                file << "TV, $800" << endl;
                break;
            case 5:
                totalAmount += 300;
                file << "Smartwatch, $300" << endl;
                break;
            default:
                cout << "Invalid choice" << endl;
                break;
        }
    }
};

// Other specific store classes can be created similarly, following the pattern above.

class Products {
private:
    double totalAmount;
    double shippingCost;
    Order currentOrder;
    string currentUser;

public:
    Products() : totalAmount(0.0), shippingCost(0.0) {}

    void setUser(const string& user) {
        currentUser = user;
    }

    void buy_sell() {
        while (true) {
        	cout<<"\nEnter Your Choice Here "<<endl;
            cout << "1. To buy products" << endl;
            cout << "2. To Sell Products" << endl;
            cout << "3. Proceed to checkout" << endl;
            cout << "4. View Order Details" << endl;
            cout << "5. Add Reviews And Comments" << endl;
            cout << "6. Exit" << endl;

            int choice;
            cout << "Choose desired command: ";
            cin >> choice;

            if (choice == 6) {
                break;
            }

            switch (choice) {
                case 1:
                    buy_products();
                    break;
                case 2:
                    sellProducts();
                    break;
                case 3:
                    checkout();
                    break;   
                case 4:
                    currentOrder.displayOrder();
                    break;
                case 5:
                    manage_reviews_comments();
                    break;
                default:
                    cout << "Invalid choice" << endl;
            }
        }
    }

   void buy_products() {
    while (true) {
        cout << "Select a store:" << endl;
        cout << " 1. Clothes Store" << endl;
        cout << " 2. Shoes Store" << endl;
        cout << " 3. Perfume Store" << endl;
        cout << " 4. Gloves store" << endl;
        cout << " 5. Beauty products store" << endl;
        cout << " 6. Electronics store" << endl;
        cout << " 7. Exit" << endl;

        int storeChoice;
        cout << "\nEnter your choice: " << endl;
        cin >> storeChoice;

        switch (storeChoice) {
            case 1:
                // store = new ClothesStore();
                break;
            case 2:
                // store = new ShoeStore();
                break;
            case 3:
                // store = new perfumeStore();
                break;
            case 4:
                // store = new glovesStore();
                break;
            case 5:
                // store = new CosmeticsStore();
                break;
            case 6:
                // store = new ElectronicsStore();
                break;
            case 7:
                return;
            default:
                // cout << "Invalid choice" << endl;
                continue;
        }

        // Uncomment the following lines when implementing the store selection
        // store->openFile("user_buy.txt");
        // store->displayProducts();
        // store->selectProduct();
        // totalAmount += store->getTotalAmount();
        // store->closeFile();
        // delete store;

        char continueShopping;
        cout << "Do you want to buy another product? (y/n): " << endl;
        cin >> continueShopping;
        if (continueShopping != 'y') {
            cout << "Would you like to cancel your order just in case?" << endl;

            string yes_no;
            cin >> yes_no;

            if (yes_no == "yes") {
                string filename = "user_buy.txt";
                string toErase = "product_name";  // Specify the product name to erase
                cancel_order(filename, toErase);
            }

            break;
        }
    }
}

    void sellProducts() {
        string product;
        double price;

        cout << "\nEnter the product you want to sell: ";
        cin >> product;

        cout << "\nEnter the price of the product: ";
        cin >> price;

        ofstream file("user_sell.txt", ios::app);
        if (file.is_open()) {
            file << product << ", $" << price << endl;
            file.close();
            cout << "Product added for sale" << endl;
        } else {
            cout << "Unable to open file for writing." << endl;
        }
    }
void checkout() {
    if (totalAmount == 0.0) {
        cout << "No items in cart to checkout." << endl;
        return;
    }

    ofstream file("user_buy.txt", ios::app);
    if (!file.is_open()) {
        cout << "Unable to open file for writing." << endl;
        return;
    }

    cout << "Proceed to checkout..." << endl;

    string shippingMethod;
    cout << "\nChoose shipping method (Standard, Express): ";
    cin >> shippingMethod;

    double shippingCost = (shippingMethod == "Express") ? 10.0: 5.0;// the ? mark shows the conditional statement 
    totalAmount += shippingCost;

    if (shippingMethod == "Standard") {
        totalAmount += 5.0; // Increase the total amount by $5 for standard shipping
    } else if (shippingMethod == "Express") {
        totalAmount += 10.0; // Increase the total amount by $10 for express shipping
    }

    static int nextOrderId = 1; // Static variable to keep track of order IDs
    currentOrder.setOrderDetails(nextOrderId++, currentUser, shippingMethod, totalAmount);

    file << "\nTotal amount to be paid: $" << totalAmount << endl;
    file << "\nShipping method: " << shippingMethod << endl;

    cout << "\nTotal amount to be paid: $" << totalAmount << endl;
    cout << "\nShipping method: " << shippingMethod << endl;

    // Resetting total amount after checkout
    totalAmount = 0.0;
}

     void manage_reviews_comments() {
        // Function to manage reviews and comments
        cout << "1. View Reviews and Comments" << endl;
        cout << "2. Leave a Review or Comment" << endl;

        int choice;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                view_reviews_comments();
                break;
            case 2:
                leave_review_comment();
                break;
            default:
                cout << "Invalid choice" << endl;
                break;
        }
    }

    void view_reviews_comments() {
        ifstream file("reviews_comments.txt");
        if (!file.is_open()) {
            cout << "No reviews or comments available." << endl;
            return;
        }

        string line;
        cout << "\nReviews and Comments:" << endl;
        while (getline(file, line)) {
            cout << line << endl;
        }

        file.close();
    }

    void leave_review_comment() {
        string review_comment;
        cout << "Enter your review or comment: "<<endl;
        cin.ignore();
        getline(cin, review_comment);

        ofstream file("reviews_comments.txt", ios::app);
        if (!file.is_open()) {
            cout << "Unable to open file for writing." << endl;
            return;
        }

        file << review_comment << endl;
        file.close();

        cout << "Thank you for your feedback!" << endl;
    }



void cancel_order(const string& filename, const string& toErase) {
    ifstream file("user_buy.txt");
    if (!file) {
        cout << "Unable to open file for reading" << endl;
        return;
    }

    stringstream buffer;
    buffer << file.rdbuf();
    string content = buffer.str();
    file.close();

    size_t pos;
    while ((pos = content.find(toErase)) != string::npos) {
        content.erase(pos, toErase.length());
    }

    ofstream fileOut("user_buy.txt");
    if (!fileOut) {
        cout << "Unable to open file for writing" << endl;
        return;
    }
    
    

    fileOut << content;
    
    cout<<"item  : "<<store<<endl;
    cout<<"Order ID : "<<nextorderID<<endl;
    
    
    
    cout<<"Order cancelled successfully"<<endl;
    fileOut.close();
}

};

int main() {
    User user;
    Products products;

    while (true) {
    	cout<<"\nChoose Your option\n";
        cout << "1. Register" << endl;
        cout << "2. Login" << endl;
        cout << "3. Exit" << endl;

        int choice;
        cout << "Choose desired option: ";
        cin >> choice;

        switch (choice) {
            case 1:
                user.registerName();
                break;
            case 2:
                if (user.loginUser()) {
                    products.buy_sell();
                }
                break;
            case 3:
                cout << "Thank you for using our services!" << endl;
                return 0;
            default:
                cout << "Invalid choice" << endl;
        }
    }

    return 0;
}
