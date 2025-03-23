# FactoryDesign Pattern

## C++ Code 
#include <iostream>
using namespace std;
class Vehicle{
    public:
    virtual void createVehicle() = 0;
};
class Car : public Vehicle{
    public:
    void createVehicle(){
        cout<<"Creating Car "<<endl;
    }
};
class Bike : public Vehicle{
    public:
    void createVehicle(){
        cout<<"Creating Bike "<<endl;
    }
};
// Client Code
int main() {
    // Write C++ code here
    string input;
    cin>>input;
    Vehicle* vehicle;
    if(input == "car"){
        vehicle = new Car();
    }
    else if(input == "Bike"){
        vehicle = new Bike();
    }
    vehicle->createVehicle();
    return 0;
}
