#include <Arduino.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define SCREEN_WIDTH 128 // largeur de l'écran
#define SCREEN_HEIGHT 64 // hauteur de l'écran
#define OLED_RESET -1 // Réinitialisation partagée de l'écran OLED
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

const int potPin = A0; // Broche du potentiomètre
const int buttonSelectPin = 2; // Broche du bouton de sélection
const int buttonBackPin = 3; // Broche du bouton retour
int menuOption = 0; // Option de menu sélectionnée
const int numOptions = 4; // Nombre d'options de menu
int lastPotValue = 0; // Dernière valeur lue du potentiomètre
int threshold = 50; // Seuil de changement pour éviter le bruit

void displayMenu(int option); // Déclaration de la fonction displayMenu
void selectOption(int option); // Déclaration de la fonction selectOption

void setup() {
  Serial.begin(115200);

  if(!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println(F("SSD1306 allocation failed"));
    for(;;);
  }

  pinMode(buttonSelectPin, INPUT_PULLUP); // Configuration du bouton de sélection avec résistance pull-up interne
  pinMode(buttonBackPin, INPUT_PULLUP); // Configuration du bouton retour avec résistance pull-up interne

  delay(2000);
  display.clearDisplay();

  display.setTextSize(1);
  display.setTextColor(WHITE);
  displayMenu(menuOption); // Afficher le menu au démarrage
}

void loop() {
  int potValue = analogRead(potPin); // Lire la valeur du potentiomètre
  
  // Si la différence entre la nouvelle et l'ancienne valeur est supérieure au seuil
  if (abs(potValue - lastPotValue) > threshold) {
    // Mettre à jour l'option de menu en fonction de la nouvelle valeur du potentiomètre
    menuOption = map(potValue, 0, 1023, 0, numOptions - 1); 
    lastPotValue = potValue; // Mettre à jour la dernière valeur lue du potentiomètre
    
    displayMenu(menuOption); // Afficher le menu mis à jour
  }

  // Vérifier si le bouton de sélection est pressé
  if (digitalRead(buttonSelectPin) == LOW) {
    selectOption(menuOption);
    delay(500); // Petit délai pour éviter les rebonds de bouton
  }

  // Vérifier si le bouton retour est pressé
  if (digitalRead(buttonBackPin) == LOW) {
    displayMenu(menuOption); // Revenir au menu principal
    delay(500); // Petit délai pour éviter les rebonds de bouton
  }

  delay(100); // Ajouter un léger délai pour rendre la navigation plus fluide
}

void displayMenu(int option) {
  display.clearDisplay();
  display.setCursor(0, 0);
  
  String menuOptions[numOptions] = {"Option 1", "Option 2", "Option 3", "Option 4"};
  
  for (int i = 0; i < numOptions; i++) {
    if (i == option) {
      display.setTextColor(BLACK, WHITE); // Mettre en surbrillance l'option sélectionnée
    } else {
      display.setTextColor(WHITE, BLACK);
    }
    display.setCursor(0, i * 10);
    display.println(menuOptions[i]);
  }
  display.display();
}

void selectOption(int option) {
  display.clearDisplay();
  display.setCursor(0, 0);
  
  if (option == 0) {
    display.println("Option 1 choisie");
  } else if (option == 1) {
    display.println("Option 2 choisie");
  } else if (option == 2) {
    display.println("Option 3 choisie");
  } else if (option == 3) {
    display.println("Option 4 choisie");
  }
  
  display.display();
}
