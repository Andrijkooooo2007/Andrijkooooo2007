#include <iostream>
#include <cstring>

class Animal {
private:
    char* species; // порода, вид
    int old; // вік тварини

public:
    // Конструктор за замовчуванням
    Animal() : species(nullptr), old(0) {}

    // Параметризований конструктор
    Animal(const char* species, int old) {
        this->species = new char[strlen(species) + 1];
        strncpy(this->species, species, strlen(species) + 1);
        this->species[strlen(species)] = '\0'; // Ensure null termination
        this->old = old;
    }

    // Конструктор копіювання
    Animal(const Animal& other) {
        species = new char[strlen(other.species) + 1];
        strncpy(species, other.species, strlen(other.species) + 1);
        species[strlen(other.species)] = '\0'; // Ensure null termination
        old = other.old;
    }

    // Метод для вводу даних
    void input() {
        char buffer[100];
        std::cout << "Введіть породу: ";
        std::cin.ignore(); // Ігноруємо залишок рядка
        std::cin.getline(buffer, sizeof(buffer)); // Використовуємо getline для вводу
        setSpecies(buffer);
        std::cout << "Введіть вік: ";
        std::cin >> old;
    }

    // Метод для виводу даних
    void print() const {
        std::cout << "Порода: " << (species ? species : "Невідомо") << ", Вік: " << old << " років" << std::endl;
    }

    // Методи доступу
    void setSpecies(const char* species) {
        delete[] this->species;
        this->species = new char[strlen(species) + 1];
        strncpy(this->species, species, strlen(species) + 1);
        this->species[strlen(species)] = '\0'; // Ensure null termination
    }

    char* getSpecies() const {
        return species;
    }

    void setOld(int old) {
        this->old = old;
    }

    int getOld() const {
        return old;
    }

    // Деструктор
    ~Animal() {
        delete[] species;
    }
};

class ListNode {
public:
    Animal data;
    ListNode* next;

    ListNode(const Animal& animal) : data(animal), next(nullptr) {}
};

class List {
private:
    ListNode* head;

public:
    List() : head(nullptr) {}

    // Додавання елемента до списку
    void add(const Animal& animal) {
        ListNode* newNode = new ListNode(animal);
        if (!head) {
            head = newNode;
        } else {
            ListNode* temp = head;
            while (temp->next) {
                temp = temp->next;
            }
            temp->next = newNode;
        }
    }

    // Виведення всіх елементів списку
    void print() const {
        ListNode* temp = head;
        while (temp) {
            temp->data.print();
            temp = temp->next;
        }
    }

    // Деструктор для очищення пам'яті
    ~List() {
        while (head) {
            ListNode* temp = head;
            head = head->next;
            delete temp;
        }
    }
};

int main() {
    List animalList;

    // Статичне створення об'єктів
    Animal dog("Собака", 5);
    Animal cat("Кіт", 3);

    animalList.add(dog);
    animalList.add(cat);

    // Динамічне створення об'єктів
    Animal* rabbit = new Animal();
    rabbit->input(); // Запит на введення даних
    animalList.add(*rabbit);

    // Вивід всіх тварин
    std::cout << "Список тварин:" << std::endl;
    animalList.print();

    delete rabbit; // Очищення динамічно виділеної пам'яті
    return 0;
}
