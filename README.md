# comidas-r-pidas-con-estructuras-en-cpp
usamos estructuras de lenguaje cpp para almacenar, generar reporte, agregar productos, etc. 
//curso: Programacion estructurada


#include <iostream>
#include <string>
#include <windows.h>
#include <conio.h>
#include <ctime>
#include <fstream>

using namespace std;
void GuardarDatos();

string currentDateTime() {
    time_t t = time(nullptr);
    tm* now = localtime(&t);
 
    char buffer[128];
    strftime(buffer, sizeof(buffer), "%m-%d-%Y %X", now);
    return buffer;
}

void gotoxy(int x,int y)
{  
	HANDLE hcon = GetStdHandle(STD_OUTPUT_HANDLE);  
	COORD dwPos;  
	dwPos.X = x;  
	dwPos.Y= y;  
	SetConsoleCursorPosition(hcon,dwPos);    
}  

void cuadro_editar_p()
{
	for(int i=7;i<94;i++)
	{
		gotoxy(i,2);
		printf("%c",205);
		gotoxy(i,4);
		printf("%c",205);
		gotoxy(i,14);
		printf("%c",205);
	}
	
	for(int i=3;i<15;i++)
	{
		gotoxy(6,i);
		printf("%c",186);
		gotoxy(94,i);
		printf("%c",186);
	}
	gotoxy(6,2);  printf("%c",201);
	gotoxy(6,14);  printf("%c",200);
	gotoxy(94,2);  printf("%c",187);
	gotoxy(94,14);  printf("%c",188);
}

void cuadro()
{
	for(int i=20;i<90;i++)
	{
		gotoxy(i,1);
		printf("%c",205);
		gotoxy(i,23);
		printf("%c",205);
	}
	
	for(int i=2;i<23;i++)
	{
		gotoxy(20,i);
		printf("%c",186);
		gotoxy(90,i);
		printf("%c",186);
	}
	gotoxy(20,1);  printf("%c",201);
	gotoxy(20,23);  printf("%c",200);
	gotoxy(90,1);  printf("%c",187);
	gotoxy(90,23);  printf("%c",188);
}

struct tres
{
	float precio;
	string caracteristicas;
	int stock;
};

struct dos
{
	string name_marca;
	int cantidad_producto;
	struct tres producto[10];
};

struct uno
{
	string name_categoria;
	int cantidad_marca;
	struct dos marca[10];
}categoria[10];

struct memoria
{
	int p_categoria;
	int p_marca;
    int p_producto;	
}posicion[10];
	
struct reporte
{
	string fecha_r;
	string nombre_cliente_r;
	int dni_r;
	int cantidad_productos_r;
	int stock_r[10];
	string caracteristicas_r[10];
	float precio_r[10];
	float ventatotal_r[10];
	
}repor_ventas[10];

int main()
{
	HANDLE hConsole = GetStdHandle( STD_OUTPUT_HANDLE );//para colores
	
	int iteracion_principal=0;
	//Categoria 1:
	categoria[0].name_categoria="Bebidas";
	categoria[0].cantidad_marca=2;
		//marca 1 de la categoria 1
		categoria[0].marca[0].name_marca="Gaseosas";
		categoria[0].marca[0].cantidad_producto=2;
		
		    //Productos de la marca 1 (lg)
		    //producto 1
		    categoria[0].marca[0].producto[0].caracteristicas="Coca Cola";
	        categoria[0].marca[0].producto[0].precio=3.5;
	        categoria[0].marca[0].producto[0].stock=3;
	        //producto 2
	        categoria[0].marca[0].producto[1].caracteristicas="Kola Escocesa";
	        categoria[0].marca[0].producto[1].precio=5;
	        categoria[0].marca[0].producto[1].stock=2;
	    
	    //marca 2 de la categoria 1
	    categoria[0].marca[1].name_marca="Refrescos";
		categoria[0].marca[1].cantidad_producto=3;
		
	        //Productos de la marca 2 (bebidas)
	        //producto 1
	        categoria[0].marca[1].producto[0].caracteristicas="Aguajina";
	        categoria[0].marca[1].producto[0].precio=2.5;
	        categoria[0].marca[1].producto[0].stock=6;
	        //producto 2
	        categoria[0].marca[1].producto[1].caracteristicas="Cocona";
	        categoria[0].marca[1].producto[1].precio=3;
	        categoria[0].marca[1].producto[1].stock=2;
	        //producto 3
	        categoria[0].marca[1].producto[2].caracteristicas="Maiz morado";
	        categoria[0].marca[1].producto[2].precio=2;
	        categoria[0].marca[1].producto[2].stock=4;
	
	//Categoria 2
	categoria[1].name_categoria="Comidas";
	categoria[1].cantidad_marca=1;
	    //Marcas
	    categoria[1].marca[0].name_marca="pizzas";
	    categoria[1].marca[0].cantidad_producto=1;
	        //Productos de la marca 1 (lg)
		    //producto 1
		    categoria[1].marca[0].producto[0].caracteristicas="Familiar";
	        categoria[1].marca[0].producto[0].precio=90;
	        categoria[1].marca[0].producto[0].stock=7;
	
	//Para la boleta
	int telefono=512456;
	int ruc=895643658;
	string fecha;
	int dni;
	string nombre_cliente;
	double suma_productos=0, sub_total, igv;
	int n_clientes=0;
	
	
	//Para estructura principal
	int accion_admin,accion_user;
	
		//Variables del caso admin
		int crear_marcas;
		int crear_productos;
		int salir_admin;
		int iteracion_nro=1;
		int salir_2=10000;
		float igv_r=0, sub_total_r=0, total_reporte=0; 
		int opcion_editar_producto_admin;
		float nuevo_precio;
		int nuevo_stock;
		//Variables del caso usuario
	    	//caso 1
	    	int op_categoria,op_marcas,op_producto;
	    	float carrito_precio[10];
			string carrito_caracteristicas[10];
			int carrito_stock[10];
			int iteracion_producto=0;//Para ver la cantidad de veces que llego hasta el final del bucle
			int num_categoria=2;  //Es igual a 2 porque solo hemos creado dos categorias
			int salir=1000; //romper bucle principal
			int cantidad_disponible;
		
			//caso 3
			int borrar_producto;
			int editar_producto;
			int opcion_editar_carrito;
			int cantidad_editar_producto;
	
		
	do
	{
		
		system("cls");
		fecha=currentDateTime();//fecha
		cout<<"\t\t\t\t\t|||||||||||||||||||||||||||||||||||||\n";
		cout<<"\t\t\t\t\t|||         Bienvenido(a) a:      |||\n";
		cout<<"\t\t\t\t\t|||           REFRIGERIOS         |||\n";
		cout<<"\t\t\t\t\t|||          LA TIA VENENO        |||\n";
		cout<<"\t\t\t\t\t|||||||||||||||||||||||||||||||||||||\n";
		
		cout<<"\n\n\t\t\t\t\t|||||||||||||||||||||||||||||||||||||\n";
		cout<<"\t\t\t\t\t|||        Ingrese su nombre:      ||\n";
		cout<<"\t\t\t\t\t|||||||||||||||||||||||||||||||||||||\n";
		
		cout<<"\n\n\t\t\t\t\t-------------------------------------\n";
		gotoxy(55,11);
		cin>>nombre_cliente;
		
		cout<<"\n\n\t\t\t\t\t|||||||||||||||||||||||||||||||||||||\n";
		cout<<"\t\t\t\t\t|||          Digite su DNI:       |||\n";
		cout<<"\t\t\t\t\t|||||||||||||||||||||||||||||||||||||\n";
		cout<<"\n\n\t\t\t\t\t---------------------------------------\n";
		gotoxy(55,18);
		cin>>dni;
		
		system("cls");
		
		//Opciones para el administrador
		if(nombre_cliente=="admin" && dni==123)
		{
			while(salir_2!=0)
			{
				system("cls");
				cout<<"----------------------------"<<endl;
				cout<<"Indique que accion desea realizar"<<endl;
				cout<<"----------------------------"<<endl;
				cout<<"1) Crear una nueva categoria de producto"<<endl;
				cout<<"2) Reporte de ventas"<<endl;
				cout<<"3) Modificar producto"<<endl;
				cout<<"9) Salir"<<endl;
			
				cin>>accion_admin;
				
				switch(accion_admin)
				{
					case 1:
						system("cls");//Para limpiar la consola
						cout<<"Ingrese el nombre de la nueva categoria"<<endl;
						cout<<"---------------------------------------"<<endl;
						fflush(stdin);
						getline(cin, categoria[num_categoria].name_categoria);
						
						system("cls");//Para limpiar la consola
						cout<<"Cuantas marcas desea crear"<<endl;
						cout<<"--------------------------"<<endl;
						cin>>categoria[num_categoria].cantidad_marca;
						
						for(int p=0; p<categoria[num_categoria].cantidad_marca; p++)
						{
						    system("cls");//Para limpiar la consola
						    fflush(stdin);
							cout<<"ingrese el nombre de la marca "<<p+1<<endl;
							getline(cin,categoria[num_categoria].marca[p].name_marca);
							
							cout<<"ingrese cuantos productos tendra la marca "<<categoria[num_categoria].marca[p].name_marca<<endl;
							cin>>categoria[num_categoria].marca[p].cantidad_producto;
							
							for(int q=0; q<categoria[num_categoria].marca[p].cantidad_producto; q++)
							{
								system("cls");//Para limpiar la consola
								
								cout<<"Ingrese el stock del producto "<<q+1<<endl;
								cin>>categoria[num_categoria].marca[p].producto[q].stock;
								
								cout<<"Ingrese el precio del producto "<<q+1<<endl;
								cin>>categoria[num_categoria].marca[p].producto[q].precio;
								
								fflush(stdin);  //para limpiar el buffer
								
								cout<<"Ingrese las caracteristicas del producto "<<q+1<<endl;
								getline(cin, categoria[num_categoria].marca[p].producto[q].caracteristicas);
							}
						}
						salir_admin=0;
						num_categoria++;
					break;
					
					case 2:
						fflush(stdin);
						system("cls");
						cout<<"\n\n\tRegistro de Ventas"<<endl;
						cout<<"\t----------------------------------------------------------------------------------------------"<<endl;
						
						cout<<"\tNro.\tFecha\t\t\tStock\tDescripcion\t\tP.unit\tTotal"<<endl;
						cout<<"\t----------------------------------------------------------------------------------------------"<<endl;
						for(int q=0; q<iteracion_principal; q++)
						{
							
							for(int z=0; z<repor_ventas[q].cantidad_productos_r; z++)
							{
								cout<<"\t"<<iteracion_nro<<")\t"<<repor_ventas[q].fecha_r<<"\t"<<repor_ventas[q].stock_r[z]<<"\t"<<repor_ventas[q].caracteristicas_r[z]<<endl;
								
								GuardarDatos();
								gotoxy(72,5+iteracion_nro);
								cout<<repor_ventas[q].precio_r[z]<<"\t"<<repor_ventas[q].ventatotal_r[z]<<endl;
								
								total_reporte+=repor_ventas[q].ventatotal_r[z];
								
								iteracion_nro++;
							}
						}
						sub_total_r=(total_reporte*82)/100;
						igv_r=(total_reporte*18)/100;
						cout<<"\t----------------------------------------------------------------------------------------------"<<endl;
						cout<<"\tSub.Total\t\t\t\t\t\t\t\t"<<sub_total_r<<endl;
						cout<<"\tIGV\t\t\t\t\t\t\t\t\t"<<igv_r<<endl;
						cout<<"\tTOTAL\t\t\t\t\t\t\t\t\t"<<total_reporte<<endl;
						cout<<"\t----------------------------------------------------------------------------------------------"<<endl;
						iteracion_nro=1;
						
						sub_total_r=0;
						igv_r=0;
						total_reporte=0;
						
						cout<<"\n\n\n";
						system("pause");
						break;
					
					case 3:
						do
						{
							cout<<"ingrese la opcion de la categoria de productos que desee"<<endl;
							cout<<"--------------------------------------------------------"<<endl;
							for(int i=0; i<num_categoria; i++)
							{
								cout<<i+1<<") "<<categoria[i].name_categoria<<endl;
							}
							cout<<"0) Regresar"<<endl;
							cin>>op_categoria; //para guardar la opcion
							
							if(op_categoria==0)
							{
								break;
							}
							
							system("cls");
							
							do
							{
								cout<<"ingrese el numero de la marca deseada"<<endl;
								cout<<"-------------------------------------"<<endl;
								for(int j=0; j<categoria[op_categoria-1].cantidad_marca; j++)
								{
									cout<<j+1<<") "<<categoria[op_categoria-1].marca[j].name_marca<<endl;
								}
								cout<<"0) Regresar"<<endl;
								
								cin>>op_marcas; //para guardar la opcion
								
								system("cls");//Para limpiar la pantalla
								
								op_producto=10000;
								
								if(op_marcas!=0)
								{
									cout<<"Ingrese el numero del producto deseado"<<endl;
									cout<<"-------------------------------------"<<endl;
									for(int k=0; k<categoria[op_categoria-1].marca[op_marcas-1].cantidad_producto; k++)
									{
										cout<<"\n"<<k+1<<") Producto "<<endl;
										cout<<"------------------------------ "<<endl;
										cout<<"\tPrecio "<<endl;
										cout<<"\t"<<categoria[op_categoria-1].marca[op_marcas-1].producto[k].precio<<endl;
									    cout<<"\tCaracteristicas "<<endl;
										cout<<"\t"<<categoria[op_categoria-1].marca[op_marcas-1].producto[k].caracteristicas<<endl;
										cout<<"\tStock "<<endl;
										cout<<"\t"<<categoria[op_categoria-1].marca[op_marcas-1].producto[k].stock<<endl;
										cout<<"------------------------------ "<<endl;
									}
									cout<<"0) Regresar"<<endl;
									
									cin>>op_producto;//para guardar la opcion
									
									cout<<"indique que accion desea realizar"<<endl;
									cout<<"---------------------------------"<<endl;
									cout<<"1) Editar precio"<<endl;
									cout<<"2) Editar stock"<<endl;
									cin>>opcion_editar_producto_admin;
									switch(opcion_editar_producto_admin)
									{
										case 1:
											cout<<"Indique el nuevo precio del producto"<<endl;
											cin>>nuevo_precio;
											
											categoria[op_categoria-1].marca[op_marcas-1].producto[op_producto-1].precio=nuevo_precio;
										break;
										
										case 2:
											cout<<"Indique el nuevo stock del producto"<<endl;
											cin>>nuevo_stock;
											
											categoria[op_categoria-1].marca[op_marcas-1].producto[op_producto-1].stock=nuevo_stock;
										break;
									}
												
							    }
						    }
							while(op_producto==0);
					    }
					    while(op_marcas==0);
					break;
						
					case 9:
				    	salir_admin=0;  //para romper bucle
				    	salir_2=0;
				    break;
					    
				    default:
				    	//cambiar color	
						SetConsoleTextAttribute(hConsole, 4);
						
				    	cout<<"|||||||||||||||||||||||||||||||||"<<endl;
				        cout<<"||Ingreso una opcion incorrecta\a||"<<endl;
				        cout<<"||     VUELVA A INTENTARLO     ||"<<endl;
				        cout<<"|||||||||||||||||||||||||||||||||"<<endl;
				        salir_admin=10000;  //para romper bucle
				        
						SetConsoleTextAttribute(hConsole, 7);
				        system("pause");
				    break;
				}
			}
			salir_2=10000;
		}
		
		//opciones para el cliente
		else
		{
			while(salir!=0)
			{
				system("cls");
				cout<<"----------------------------"<<endl;
				cout<<"Indique que accion desea realizar"<<endl;
				cout<<"----------------------------"<<endl;
				cout<<"1) Agregar productos al carrito"<<endl;
				cout<<"2) Comprar los productos del carrito"<<endl;
				cout<<"3) Editar productos del carrito"<<endl;
				cout<<"9) Salir"<<endl;
				cin>>accion_user;
				
				system("cls");
				
				switch(accion_user)
				{
					case 1:
						//MOSTRAR TODOS LOS PRODUCTOS Y DE ESTE GUARDAR SU PRECIO Y CARACTERISTICAS Y CANTIDAD
						do
						{
							cout<<"ingrese la opcion de la categoria de productos que desee"<<endl;
							cout<<"--------------------------------------------------------"<<endl;
							for(int i=0; i<num_categoria; i++)
							{
								cout<<i+1<<") "<<categoria[i].name_categoria<<endl;
							}
							cout<<"0) Regresar"<<endl;
							cin>>op_categoria; //para guardar la opcion
							
							if(op_categoria==0)
							{
								break;
							}
							
							system("cls");
							
							do
							{
								cout<<"ingrese el numero de la marca deseada"<<endl;
								cout<<"-------------------------------------"<<endl;
								for(int j=0; j<categoria[op_categoria-1].cantidad_marca; j++)
								{
									cout<<j+1<<") "<<categoria[op_categoria-1].marca[j].name_marca<<endl;
								}
								cout<<"0) Regresar"<<endl;
								
								cin>>op_marcas; //para guardar la opcion
								
								system("cls");//Para limpiar la pantalla
								
								op_producto=10000;
								
								if(op_marcas!=0)
								{
									cout<<"Ingrese el numero del producto deseado"<<endl;
									cout<<"-------------------------------------"<<endl;
									for(int k=0; k<categoria[op_categoria-1].marca[op_marcas-1].cantidad_producto; k++)
									{
										cout<<"\n"<<k+1<<") Producto "<<endl;
										cout<<"------------------------------ "<<endl;
										cout<<"\tPrecio "<<endl;
										cout<<"\t"<<categoria[op_categoria-1].marca[op_marcas-1].producto[k].precio<<endl;
									    cout<<"\tCaracteristicas "<<endl;
										cout<<"\t"<<categoria[op_categoria-1].marca[op_marcas-1].producto[k].caracteristicas<<endl;
										cout<<"\tStock "<<endl;
										cout<<"\t"<<categoria[op_categoria-1].marca[op_marcas-1].producto[k].stock<<endl;
										cout<<"------------------------------ "<<endl;
									}
									cout<<"0) Regresar"<<endl;
									
									cin>>op_producto;//para guardar la opcion
									
									//restricciones
									do
									{
									
										if(op_producto<0 || op_producto>categoria[op_categoria-1].marca[op_marcas-1].cantidad_producto)
										{
											while(op_producto<0 || op_producto>categoria[op_categoria-1].marca[op_marcas-1].cantidad_producto)
											{
												cout<<"|||||||||||||||||||||||||||||||||"<<endl;
										        cout<<"||Ingreso una opcion incorrecta\a||"<<endl;
										        cout<<"||     VUELVA A INTENTARLO     ||"<<endl;
										        cout<<"|||||||||||||||||||||||||||||||||"<<endl;
										        
										        cout<<"\nIngrese otra vez el numero del producto o puede regresar"<<endl;
										        cin>>op_producto;
									    	}
										}
										if(categoria[op_categoria-1].marca[op_marcas-1].producto[op_producto-1].stock==0)
										{
											cout<<"|||||||||||||||||||||||||||||||||"<<endl;
									        cout<<"||       NO HAY STOCK :c       \a||"<<endl;
									        cout<<"|||||||||||||||||||||||||||||||||"<<endl;
									        
									        cout<<"\nIngrese otra vez el numero del producto o puede regresar"<<endl;
									        cin>>op_producto;
										}
									}
									while((categoria[op_categoria-1].marca[op_marcas-1].producto[op_producto-1].stock)==0);
									
									//La operacion de la funcion
									if(op_producto>0)
									{
										//restriccion stock
										do
										{
											cout<<"Cuantos productos desea comprar"<<endl;
											cin>>cantidad_disponible;
											
											if(cantidad_disponible>categoria[op_categoria-1].marca[op_marcas-1].producto[op_producto-1].stock)
											{
												cout<<"No hay stock disponible"<<endl;
											}
										}
										while(cantidad_disponible>categoria[op_categoria-1].marca[op_marcas-1].producto[op_producto-1].stock);
									
									
										//operaciones
										
										carrito_precio[iteracion_producto]=categoria[op_categoria-1].marca[op_marcas-1].producto[op_producto-1].precio;
									    carrito_caracteristicas[iteracion_producto]=categoria[op_categoria-1].marca[op_marcas-1].producto[op_producto-1].caracteristicas;
									    carrito_stock[iteracion_producto]=cantidad_disponible;
									    
									    //Para guardar posicion de memoria del producto
										posicion[iteracion_producto].p_categoria=op_categoria-1;
										posicion[iteracion_producto].p_marca=op_marcas-1;
										posicion[iteracion_producto].p_producto=op_producto-1;
									    
									    //Para restar la cantidad comprada del almacen
									    categoria[op_categoria-1].marca[op_marcas-1].producto[op_producto-1].stock-=cantidad_disponible;
									    
										iteracion_producto++;
									
									}
									else
									{
										op_producto=0;
										system("cls");//Para limpiar la pantalla
									}			
							    }
						    }
							while(op_producto==0);
					    }
					    while(op_marcas==0);
				    break;
				    
				    case 2:
				    	if(iteracion_producto==0)
				    	{
				    		SetConsoleTextAttribute(hConsole, 6);
				    		cout<<"|||||||||||||||||||||||||||||||||||||||"<<endl;
					        cout<<"||NO HAY NI UN PRODUCTO EN EL CARRITO\a||"<<endl;
					        cout<<"|||||||||||||||||||||||||||||||||||||||"<<endl;
							SetConsoleTextAttribute(hConsole, 7);
							system("pause");	
						}
						else
						{
							
					    	system("cls");
					    
					    	
					    	cout<<"\n\t\t\t---------------------------------------------------------------"<<endl;
					    	SetConsoleTextAttribute(hConsole, 3); //color
					    	cout<<"\n\t\t\t\t\t   REFRIGERIOS LA TIA VENENO"<<endl;
					    	SetConsoleTextAttribute(hConsole, 7); //color
					    	cout<<"\n\t\t\t---------------------------------------------------------------"<<endl;
							cout<<"\t\t\tTelefono: \t"<<telefono<<endl;
							cout<<"\t\t\tRuc: \t\t"<<ruc<<endl;
							cout<<"\t\t\tFecha: \t\t"<<fecha<<endl;
							cout<<"\t\t\t---------------------------------------------------------------"<<endl;
							cout<<"\t\t\tNombre: \t"<<nombre_cliente<<endl;
							cout<<"\t\t\tDNI: \t\t"<<dni<<endl;
							cout<<"\t\t\t---------------------------------------------------------------"<<endl;
							cout<<"\t\t\tCant.\tDescripcion\t\t\t\tP.unit\tTOTAL"<<endl;
							cout<<"\t\t\t---------------------------------------------------------------"<<endl;
							
							for(int m=0; m<iteracion_producto; m++)
							{
								cout<<"\t\t\t"<<carrito_stock[m]<<"\t"<<carrito_caracteristicas[m]<<endl;
								
								gotoxy(72,15+m);
								cout<<carrito_precio[m]<<"\t"<<carrito_stock[m]*carrito_precio[m]<<endl;
							
								suma_productos+=carrito_stock[m]*carrito_precio[m];
							}
					
							sub_total=(suma_productos*82)/100;
							igv=(suma_productos*18)/100;
			
							cout<<"\t\t\t---------------------------------------------------------------"<<endl;
							cout<<"\t\t\tSUB.TOTAl\t\t\t\t\t"<<"s/.\t"<<sub_total<<endl;
							cout<<"\t\t\tI.G.V\t\t\t\t\t\t"<<"s/.\t"<<igv<<endl;
							cout<<"\t\t\tTOTAL\t\t\t\t\t\t"<<"s/.\t"<<suma_productos<<endl;
							cout<<"\t\t\t---------------------------------------------------------------"<<endl;
							SetConsoleTextAttribute(hConsole, 3);
							cout<<"\n\n\t\t\t\t\t  GRACIAS POR SU PREFERENCIA"<<endl;
							SetConsoleTextAttribute(hConsole, 7);
							cout<<"\n\n\t\t\t---------------------------------------------------------------"<<endl;
							//cuadro();//El marco de la boleta
							
							cout<<"\n"<<endl;
							
							//Para reporte de ventas
							repor_ventas[n_clientes].fecha_r=fecha;
							repor_ventas[n_clientes].nombre_cliente_r=nombre_cliente;
							repor_ventas[n_clientes].dni_r=dni;
							repor_ventas[n_clientes].cantidad_productos_r=iteracion_producto;
							
							for(int a=0; a<iteracion_producto; a++)
							{
								repor_ventas[n_clientes].stock_r[a]=carrito_stock[a];
								repor_ventas[n_clientes].caracteristicas_r[a]=carrito_caracteristicas[a];
								repor_ventas[n_clientes].precio_r[a]=carrito_precio[a];
								repor_ventas[n_clientes].ventatotal_r[a]=carrito_stock[a]*carrito_precio[a];
							}
							
							//reiniciamos valores
							iteracion_producto=0;
							sub_total=0;
							igv=0;
							suma_productos=0;
							
							//la cantidad de clientes
							n_clientes++;
							
							system("pause");
							system("cls");
							
					    	salir=0; //para romper el bucle principal
					    	salir_admin=0;
					    	iteracion_principal++;
					    }
				    break;
				    
				    case 3:
				    	//restriccion
				    	if(iteracion_producto==0)
				    	{
				    		SetConsoleTextAttribute(hConsole, 6);//para cambiar el color
				    		cout<<"|||||||||||||||||||||||||||||||||||||||"<<endl;
					        cout<<"||NO HAY NI UN PRODUCTO EN EL CARRITO\a||"<<endl;
					        cout<<"|||||||||||||||||||||||||||||||||||||||"<<endl;
							SetConsoleTextAttribute(hConsole, 7);
							system("pause");
							
						}
						
						//funcion
					    else
					    {
					    	
						    cout<<"\n\n\n\t\tCANTIDAD\tDESCRIPCION\t\t\t\tPRECIO\t\tTOTAL"<<endl;
							
					    	for(int m=0; m<iteracion_producto; m++)
							{
								cout<<"\n\t"<<m+1<<")\t"<<carrito_stock[m]<<"\t\t"<<carrito_caracteristicas[m]<<endl;
							
							}
							for(int m=0; m<iteracion_producto; m++)
							{
								gotoxy(72,5+(2*m));
								cout<<carrito_precio[m]<<"\t\t"<<carrito_stock[m]*carrito_precio[m]<<endl;
							
							}
							
							//cuadro_editar_p(); /El marco
							
							cout<<"\n\nIndique que accion desea realizar"<<endl;
							cout<<"---------------------------------"<<endl;
							cout<<"1) Borrar el producto"<<endl;
							cout<<"2) Editar cantidad"<<endl;
							cout<<"3) Regresar"<<endl;
							
							cin>>opcion_editar_carrito;
							
							switch(opcion_editar_carrito)
							{
								case 1:
									cout<<"Indique el producto a borrar"<<endl;
									cin>>borrar_producto;
									
									//borra el producto
									for(int u=(borrar_producto-1); u<iteracion_producto; u++)
									{
										carrito_precio[u]=carrito_precio[u+1];
										carrito_caracteristicas[u]=carrito_caracteristicas[u+1];
									}
									
									iteracion_producto--;
									
									//Para regresar el stock
									categoria[posicion[borrar_producto-1].p_categoria].marca[posicion[borrar_producto-1].p_marca].producto[posicion[borrar_producto-1].p_producto].stock+=carrito_stock[borrar_producto-1];								
								break;
								
								case 2:
									cout<<"Indique el producto a editar"<<endl;
									cin>>editar_producto;
									
									cout<<"Indique la nueva cantidad a comprar"<<endl;
									cin>>cantidad_editar_producto;
									
									//restriccion
									if(cantidad_editar_producto<=0 || cantidad_editar_producto>carrito_stock[editar_producto-1])
									{
										while(cantidad_editar_producto<=0 || cantidad_editar_producto>carrito_stock[editar_producto-1])
										{
											cout<<"No se puede realizar esa operacion intentelo otra vez"<<endl;
											cout<<"-----------------------------------------------------"<<endl;
											cout<<"Indique la nueva cantidad a comprar"<<endl;
											cin>>cantidad_editar_producto;
										}
									}
									
									//Para regresar y/o devolver el stock
									categoria[posicion[editar_producto-1].p_categoria].marca[posicion[editar_producto-1].p_marca].producto[posicion[editar_producto-1].p_producto].stock+=(carrito_stock[editar_producto-1]-cantidad_editar_producto);
									
									carrito_stock[editar_producto-1]=cantidad_editar_producto;
								break;
								
								case 3:
									salir=1000;
								break;
								
								default:
									//cambiar color	
									SetConsoleTextAttribute(hConsole, 4);
									
							    	cout<<"|||||||||||||||||||||||||||||||||"<<endl;
							        cout<<"||Ingreso una opcion incorrecta\a||"<<endl;
							        cout<<"||     VUELVA A INTENTARLO     ||"<<endl;
							        cout<<"|||||||||||||||||||||||||||||||||"<<endl;
							        
									SetConsoleTextAttribute(hConsole, 7);
							        system("pause");
								break;
							}
							
					    }
				    break;
				    
				    case 9:
				    	salir_admin=0;  //para romper bucle
				        salir=0; //para romper el bucle principal
				    break;
				    
				    default:
				    	//cambiar color	
						SetConsoleTextAttribute(hConsole, 4);
						
				    	cout<<"|||||||||||||||||||||||||||||||||"<<endl;
				        cout<<"||Ingreso una opcion incorrecta\a||"<<endl;
				        cout<<"||     VUELVA A INTENTARLO     ||"<<endl;
				        cout<<"|||||||||||||||||||||||||||||||||"<<endl;
				        salir_admin=10000;  //para romper bucle
				        
						SetConsoleTextAttribute(hConsole, 7);
				        system("pause");
				        
				    break;
			    }
			}
			salir=1000;	
	    }
    }
    while(salir_admin==0);
}
