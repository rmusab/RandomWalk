/************************************************
* Random walk on a stratified set               *
* By Ravil Mussabayev                           *
* ravmus@gmail.com                              *
************************************************/
#include <stdlib.h>
//#include <stdbool.h>
#include <GL/glut.h>
#include <random>
#include <cstring>

#define SCREEN_WIDTH 750
#define SCREEN_HEIGHT 750
#define POINTS_NUMBER 100000
#define STEPS_NUMBER 50000
#define X0 0.0
#define Y0 0.0
#define DELTA 0.001

using namespace std;

struct Point {
        double x, y;
        };

Point* points;
int n;
default_random_engine e;
uniform_real_distribution<double> d(0, 1);

bool fullscreen = false;

bool init()
{
	return true;
}

void display()
{
    points = new Point[POINTS_NUMBER];
    for (int i = 0; i < POINTS_NUMBER; i++) {
        points[i].x = X0;
        points[i].y = Y0;
    };

    n = 0; e.seed(time(0));
}

void print(double x, double y, double z, char *text)
{
glRasterPos2f(x,y);
int len = (int) strlen(text);

for (int i = 0; i < len; i++)
{
glutBitmapCharacter(GLUT_BITMAP_TIMES_ROMAN_24, text[i]);
}
};

void idle() {
    glClearColor(0.0f, 0.0f, 0.0f, 0.0f);
	glClear(GL_COLOR_BUFFER_BIT);

	glMatrixMode(GL_PROJECTION);
    glLoadIdentity();
    glMatrixMode(GL_MODELVIEW);
    glLoadIdentity();

    glLineWidth(1);
    glColor3f(0, 0, 1);
    glBegin(GL_LINES);
    glVertex3f(0, -1, 0);
    glVertex3f(0, 1, 0);
    glVertex3f(-1, 0, 0);
    glVertex3f(1, 0, 0);
    glEnd();

    ++n;

       for (int i = 0; i < POINTS_NUMBER; i++) {
            double alpha = d(e);

            if ((abs(points[i].x) <= DELTA) && (abs(points[i].y) <= DELTA)) {

                if (alpha < 0.25) points[i].x -= DELTA;
                 else if (alpha < 0.5) points[i].x += DELTA;
                  else if (alpha < 0.75) points[i].y -= DELTA;
                    else points[i].y += DELTA;

            } else if (abs(points[i].y) <= 0) {

                double delta1 = DELTA / (2 + 2 * DELTA);
                double delta2 = 1 / (2 + 2 * DELTA);
                if (alpha < delta1) points[i].y += DELTA;
                 else if (alpha < (2 * delta1)) points[i].y -= DELTA;
                  else if (alpha < (2 * delta1 + delta2)) points[i].x -= DELTA;
                   else points[i].x += DELTA;

            } else if (abs(points[i].x) <= 0) {

                double delta1 = DELTA / (2 + 2 * DELTA);
                double delta2 = 1 / (2 + 2 * DELTA);
                if (alpha < delta1) points[i].x += DELTA;
                 else if (alpha < (2 * delta1)) points[i].x -= DELTA;
                  else if (alpha < (2 * delta1 + delta2)) points[i].y -= DELTA;
                   else points[i].y += DELTA;

            } else {
                if (alpha < 0.25) points[i].x -= DELTA;
                 else if (alpha < 0.5) points[i].x += DELTA;
                  else if (alpha < 0.75) points[i].y -= DELTA;
                    else points[i].y += DELTA;
                    }
            };

    glColor3f(0, 1, 0);
    glPointSize(1.0);
    glBegin(GL_POINTS);
    for (int i = 0; i < POINTS_NUMBER; i++)
        glVertex3f(points[i].x, points[i].y, 0);
    glEnd();

    if (n == STEPS_NUMBER) glutIdleFunc(NULL);
    glColor3f(1, 0, 0);

    string s = to_string(n);
    char const *str_n = s.c_str();
    char text[100] = "n = ";
    strcat(text, str_n);
    print(-0.9, 0.9, 0, text);
    fprintf(stdout, "n = %5d\n", n);

	glFlush();
	glutSwapBuffers();
}

void keyboard(unsigned char key, int x, int y)
{
	if (key == 27)
		exit(1);
}

void specialKeyboard(int key, int x, int y)
{
	if (key == GLUT_KEY_F1)
	{
		fullscreen = !fullscreen;

		if (fullscreen)
			glutFullScreen();
		else
		{
			glutReshapeWindow(SCREEN_WIDTH, SCREEN_HEIGHT);
			glutPositionWindow(0, 0);
		}
	}
}

int main(int argc, char *argv[])
{
	glutInit(&argc, argv);

	glutInitWindowPosition(0, 0);
	glutInitWindowSize(SCREEN_WIDTH, SCREEN_HEIGHT);

	glutInitDisplayMode(GLUT_RGB | GLUT_DOUBLE);

	glutCreateWindow("03 - OpenGL Window");

	glutDisplayFunc(display);
	glutIdleFunc(idle);
	glutKeyboardFunc(keyboard);
	glutSpecialFunc(specialKeyboard);

	if (!init())
		return 1;

	glutMainLoop();

	return 0;
}
