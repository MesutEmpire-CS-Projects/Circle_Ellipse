#include <GL/glut.h>
#include <iostream>
#include <cmath>
#include <vector>
using namespace std;


struct Vertex {
    double x ;
    double y ;
};

vector<Vertex> coordinates ;
double x_0,y_0,r_x,r_y;


vector<Vertex> BresenhamEllipse(double x , double y , double a , double b){
    double x_c,y_c,p1_k,p2_k,mg1,mg2;
    mg1 = 2 * pow(b,2)*x;
    mg2 = 2 * pow(a,2) * y ;
    p1_k = pow(b,2) - pow(a,2)*b +(0.25)*pow(a,2);

    //Initialize the origin coordinates
    x_c = x;
    y_c = y;

    x=0;
    y=b;

    // Calculation for Region 1
    while (mg1 <= mg2){
        // Initialize the vertex with coordinates of the x and y in
        // all the octants
        Vertex val;
        val.x = x_c + x;
        val.y = y_c + y ;
        coordinates.push_back(val);
        val.x = x_c -x;
        val.y =y_c + y ;
        coordinates.push_back(val);
        val.x = x_c - x;
        val.y = y_c - y ;
        coordinates.push_back(val);
        val.x = x_c + x;
        val.y = y_c - y ;
        coordinates.push_back(val);

        // Increment x
        x += 1;

        // Midpoint is inside the ellipse
        if (p1_k < 0 ){
            p1_k += 2 * pow(b,2) * x + pow(b,2);
        }
            // Midpoint is outside or at the boundary ellipse
        else {
            y -=1;
            p1_k += 2 * pow(b,2) * x  - 2 * pow(a,2) * y  + pow(b,2);
        }

        // Update the gradient of the targents
        mg1 = 2 * pow(b,2)*x;
        mg2 = 2 * pow(a,2) * y ;
    }

    p2_k = pow(b,2) * pow((x + (0.5)),2) + pow(a,2) * pow((y - 1 ),2) - pow(a,2)*pow(b,2);


    // Calculation for Region 2
    while (x <= a and y >= 0){
        // Initialize the vertex with coordinates of the x and y in
        // all the octants
        Vertex val;
        val.x = x_c + x;
        val.y = y_c + y ;
        coordinates.push_back(val);
        val.x = x_c - x;
        val.y = y_c + y ;
        coordinates.push_back(val);
        val.x = x_c - x;
        val.y = y_c - y ;
        coordinates.push_back(val);
        val.x = x_c + x;
        val.y = y_c - y ;
        coordinates.push_back(val);

        //decrement y
        y -= 1;

        // Midpoint is inside the ellipse
        if (p2_k > 0 ){
            p2_k -=2 * pow(a,2) * y + pow(a,2);
        }
            // Midpoint is outside or at the boundary ellipse
        else {
            x +=1;
            p2_k +=2 * pow(b,2) * x - 2 * pow(a,2) * y  + pow(a,2);
        }

        // Update the gradient of the targents
        mg1 = 2 * pow(b,2)*x;
        mg2 = 2 * pow(a,2) * y ;

    }



    return coordinates;
}


void draw(){
    vector<Vertex> points = BresenhamEllipse(x_0,y_0,r_x,r_y);
    glClear(GL_COLOR_BUFFER_BIT);

    //Change color to black
    glColor3f(0.0,0.0,0.0);

    //Draw X - Axes
    glBegin(GL_LINES);
    glVertex2f(0,-20);
    glVertex2f(0,20);
    glEnd();

    //Draw Y - Axes
    glBegin(GL_LINES);
    glVertex2f(-20,0);
    glVertex2f(20,0);
    glEnd();

    // Draw the Ellipse
    glPointSize(15.0);
    glBegin(GL_POLYGON);
    glColor3f(0.0,1.0,0.0);
    for (const Vertex& val : points){
        cout << "( " << val.x <<" , " << val.y << " )" << endl;
        glVertex2d(val.x,val.y);
    }
    glEnd();
    glFlush();
}

void init(){
    glClearColor(1.0, 1.0, 1.0, 0.0); // Background to white
    gluOrtho2D(-20, 20, -20, 20);
}

int main (int argc,char** argv){
    // Take user input on the origin and radius of the circle
    cout << "Enter the coordinates of the origin : "<< endl;
    cout << "X :" << endl;
    cin >> x_0;
    cout << "Y :" << endl;
    cin >> y_0;
    cout << "Rx :" <<endl;
    cin >> r_x;
    cout << "Ry :" <<endl;
    cin >> r_y;

    glutInit(&argc,argv);
    glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
    glutInitWindowSize(800,800);
    glutCreateWindow("Bresenham Ellipse");
    init();
    glutDisplayFunc(draw);
    glutMainLoop();

}