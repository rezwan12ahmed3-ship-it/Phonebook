# Phonebook
Author- Rezwan Ahmed

#include <iostream>
#include <vector>
#include <fstream> 
using namespace std;

struct Contact
{
    string name, phone, email;
};

void saveContacts(const vector<Contact> &contacts)
{
    ofstream file("contacts.txt"); 
    for (auto &c : contacts)
    {
        file << c.name << "\n"
             << c.phone << "\n"
             << c.email << "\n";
    }
    file.close();
}


void loadContacts(vector<Contact> &contacts)
{
    ifstream file("contacts.txt"); 
    if (!file.is_open())
        return; 

    Contact c;
    while (getline(file, c.name) && getline(file, c.phone) && getline(file, c.email))
    {
        contacts.push_back(c);
    }
    file.close();
}

int main()
{
    vector<Contact> contacts;
    int ch, idx;
    string name, phone, email;

    
    loadContacts(contacts);

    do
    {
        cout << "\n--- Phone Book ---\n"
             << "1. Add contact\n2. View contact\n3. Search contact\n4. Edit contact\n5. Delete contact\n6. Total Contacts\n0. Exit\nEnter your choice (0-6): ";
        cin >> ch;
        cin.ignore();

        if (ch == 1)
        {
            cout << "\n--- Add Contact ---\n";
            cout << "Name: ";
            getline(cin, name);
            cout << "Phone: ";
            getline(cin, phone);
            cout << "Email(optional): ";
            getline(cin, email);
            contacts.push_back({name, phone, email});
            cout << "Contact added successfully!\n";
            saveContacts(contacts); 
        }
        else if (ch == 2)
        {
            cout << "\n--- All Contacts ---\n";
            if (contacts.empty())
                cout << "No contacts.\n";
            else
                for (int i = 0; i < contacts.size(); i++)
                    cout << i + 1 << ". " << contacts[i].name
                         << " | " << contacts[i].phone
                         << " | " << contacts[i].email << "\n";
        }
        else if (ch == 3)
        {
            cout << "\n--- Search Contact ---\n";
            cout << "Enter name to search: ";
            getline(cin, name);
            bool found = false;
            for (auto &c : contacts)
                if (c.name == name)
                {
                    cout << "Found: " << c.name << " | " << c.phone << " | " << c.email << "\n";
                    found = true;
                }
            if (!found)
                cout << "Not found.\n";
        }
        else if (ch == 4)
        {
            cout << "\n--- Edit Contact ---\n";
            cout << "Enter contact number to edit: ";
            cin >> idx;
            cin.ignore();
            if (idx > 0 && idx <= contacts.size())
            {
                cout << "New Name (old: " << contacts[idx - 1].name << "): ";
                getline(cin, name);
                cout << "New Phone (old: " << contacts[idx - 1].phone << "): ";
                getline(cin, phone);
                cout << "New Email (old: " << contacts[idx - 1].email << "): ";
                getline(cin, email);
                if (!name.empty())
                    contacts[idx - 1].name = name;
                if (!phone.empty())
                    contacts[idx - 1].phone = phone;
                if (!email.empty())
                    contacts[idx - 1].email = email;
                cout << "Contact updated!\n";
                saveContacts(contacts); 
            }
            else
                cout << "Invalid index!\n";
        }
        else if (ch == 5)
        {
            cout << "\n--- Delete Contact ---\n";
            cout << "Enter contact number to delete: ";
            cin >> idx;
            cin.ignore();
            if (idx > 0 && idx <= contacts.size())
            {
                contacts.erase(contacts.begin() + idx - 1);
                cout << "Contact deleted!\n";
                saveContacts(contacts); 
            }
            else
                cout << "Invalid index!\n";
        }
        else if (ch == 6)
        {
            cout << "\n--- Total Contacts ---\n";
            cout << "Total contacts: " << contacts.size() << "\n";
        }
    } while (ch != 0);

    cout << "Exiting... All contacts saved.\n";
}
