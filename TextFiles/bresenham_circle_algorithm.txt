#include <GL/glut.h>
#include <iostream>
#include <vector>
using namespace std;

// for holding the x and y coordinate
struct Vertex {
    double x;
    double y;
};

vector<Vertex> coordinates;

double x_0,y_0,radius;

vector<Vertex> BresenhamAlgorithmn(double x, double y, double r){
    double pk,x_c,y_c;

    //Initialize the origin coordinates
    x_c = x;
    y_c = y;

    x=0;
    y=r;

    cout << x_c<<endl;
    cout << y_c<<endl;

    //Initialize the deci                                          sion perimeter
    pk = 3-2*r;

    while (y>=x){
        cout << x_c<<endl;
        cout << y_c<<endl;

        // Initialize the vertex with coordinates of the x and y in
        // all the quadrants
        Vertex val;
        val.x = x_c + x;
        val.y = y_c + y ;
        coordinates.push_back(val);
        val.x = x_c -x;
        val.y =y_c+ y ;
        coordinates.push_back(val);
        val.x = x_c-x;
        val.y = y_c-y ;
        coordinates.push_back(val);
        val.x = x_c + x;
        val.y = y_c-y ;
        coordinates.push_back(val);

        // If x is not same as y meaning the coordinates are not being repeated
        if(x!=y){
            val.x = x_c + y;
            val.y = y_c + x ;
            coordinates.push_back(val);
            val.x = x_c + y;
            val.y = y_c-x ;
            coordinates.push_back(val);
            val.x = x_c-y;
            val.y = y_c-x ;
            coordinates.push_back(val);
            val.x = x_c-y;
            val.y = y_c + x ;
            coordinates.push_back(val);
        }

           // The midpoint is then outside the perimeter
        if (pk > 0 ) {
            pk += 4 *(x-y) +10 ;
            y-=1;

        }
        // If decision parameter < 0 then inside or in the perimeter
        else {
            pk += 4 * x + 6 ;
        }

        x+=1;


    }
    return coordinates;
}


void draw(){
    vector<Vertex> points = BresenhamAlgorithmn(x_0,y_0,radius);
    glClear(GL_COLOR_BUFFER_BIT);

    //Change color to black
    glColor3f(0.0,0.0,0.0);

    //Draw X - Axes
    glBegin(GL_LINES);
    glVertex2f(0,-10);
    glVertex2f(0,10);
    glEnd();

    //Draw Y - Axes
    glBegin(GL_LINES);
    glVertex2f(-10,0);
    glVertex2f(10,0);
    glEnd();

    // Draw the Circle
    glPointSize(15.0);
    glBegin(GL_POLYGON);
    glColor3f(1.0,0.0,0.0);
    for (const Vertex& val : points){
        cout << "( " << val.x <<" , " << val.y << " )" << endl;
        glVertex2d(val.x,val.y);
    }
    glEnd();

    // Draw the diameter of the circle
    glBegin(GL_LINES);
    glColor3f(0.0, 0.0,0.0);
    glVertex2d(x_0-radius, y_0);
    glVertex2d(x_0+radius, y_0);
    glEnd();

    glFlush();
}

void init(){
    glClearColor(1.0, 1.0, 1.0, 0.0); // Background to white
    gluOrtho2D(-10, 10, -10, 10);
}


int main(int argc, char** argv){
    // Take user input on the origin and radius of the circle
    cout << "Enter the coordinates of the origin : "<< endl;
    cout << "X :" << endl;
    cin >> x_0;
    cout << "Y :" << endl;
    cin >> y_0;
    cout << "Radius :" <<endl;
    cin >> radius;

    //Initialize glut
    glutInit(&argc,argv);
    glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
    glutInitWindowSize(800,800); //size of window
    glutInitWindowPosition(100,150); // position of window
    glutCreateWindow("Bresenham Circle"); // name of the window
    init(); // user init function
    glutDisplayFunc(draw);
    glutMainLoop();

    return 0;
}