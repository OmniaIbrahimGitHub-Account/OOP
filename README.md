# OOP Project
 It is a Pharmacy System
#include <iostream>
#include <vector>
#include <fstream>

using namespace std;

class Drug {
public:
    string name;
    int quantity;
    double price;
    string expiryDate;

    // Constructor
    Drug(string n, int q, double p, string e) {
        name = n;
        quantity = q;
        price = p;
        expiryDate = e;
    }

    // Make the display function const so that it can be called on const objects
    void display() const {
        cout << "Drug Name: " << name << "\nQuantity: " << quantity
             << "\nPrice: " << price << "\nExpiry Date: " << expiryDate << "\n";
    }
};

void saveDrugsToFile(const vector<Drug>& drugs) {
    ofstream file("pharmacy.txt");
    if (!file) {
        cout << "Error opening file for writing!\n";
        return;
    }
    for (const auto& drug : drugs) {
        file << drug.name << " " << drug.quantity << " "
             << drug.price << " " << drug.expiryDate << "\n";
    }
    file.close();
}

void loadDrugsFromFile(vector<Drug>& drugs) {
    ifstream file("pharmacy.txt");
    if (!file) {
        cout << "Data file not found. A new file will be created upon saving.\n";
        return;
    }
    string name, expiry;
    int quantity;
    double price;

    while (file >> name >> quantity >> price >> expiry) {
        drugs.push_back(Drug(name, quantity, price, expiry));
    }
    file.close();
}

void addDrug(vector<Drug>& drugs) {
    string name, expiry;
    int quantity;
    double price;

    cout << "Enter the drug name: ";
    cin >> name;
    cout << "Enter the quantity: ";
    cin >> quantity;
    cout << "Enter the price: ";
    cin >> price;
    cout << "Enter the expiry date: ";
    cin >> expiry;

    drugs.push_back(Drug(name, quantity, price, expiry));
    saveDrugsToFile(drugs);
    cout << "Drug added successfully!\n";
}

void displayDrugs(const vector<Drug>& drugs) {
    if (drugs.empty()) {
        cout << "No drugs in stock.\n";
        return;
    }

    for (const auto& drug : drugs) {
        drug.display();
        cout << "------------------------\n";
    }
}

void deleteDrug(vector<Drug>& drugs, const string& drugName) {
    bool found = false;
    vector<Drug> updatedDrugs;

    for (const auto& drug : drugs) {
        if (drug.name != drugName) {
            updatedDrugs.push_back(drug);
        } else {
            found = true;
        }
    }

    if (found) {
        drugs = updatedDrugs;
        saveDrugsToFile(drugs);
        cout << "Drug deleted successfully!\n";
    } else {
        cout << "Drug not found.\n";
    }
}

int main() {
    vector<Drug> drugs;
    loadDrugsFromFile(drugs);

    int choice;
    while (true) {
        cout << "\n1- Add Drug\n2- Display Drugs\n3- Delete Drug\n4- Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        if (choice == 1) {
            addDrug(drugs);
        } else if (choice == 2) {
            displayDrugs(drugs);
        } else if (choice == 3) {
            string name;
            cout << "Enter the name of the drug to delete: ";
            cin >> name;
            deleteDrug(drugs, name);
        } else if (choice == 4) {
            cout << "Exiting...\n";
            break;
        } else {
            cout << "Invalid option, please try again.\n";
        }
    }

    return 0;
}
