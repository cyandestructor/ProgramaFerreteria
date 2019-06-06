#include<iostream>
#include<iomanip>
#include<locale>
#include<string>
#include<algorithm>
#include<wctype.h>
#include<fstream>

using namespace std;

//DEFINICION DE ESTRUCTURAS

struct Product {

	wstring cod;
	wstring name;
	float price = 0;
	int cant = 0;

}Productos[100]; //EL NUMERO MAXIMO DE PRODUCTOS QUE SE PODRAN INGRESAR ES 100

struct Venta {

	wstring cod_prod;
	wstring product;
	int cant = 0;
	float price_x_unit = 0;
	float total = 0;

}Ventas[200]; //EL NUMERO MAXIMO DE VENTAS DIARIAS ES 200

//PROTOTIPOS DE FUNCIONES

int LookForSpace();											//FUNCION QUE BUSCA ESPACIO EN EL ARREGLO Productos
int LookForSalesSpace();									//FUNCION QUE BUSCA ESPACIO EN EL ARREGLO Ventas
int SearchProduct(wstring busq);							//FUNCION QUE BUSCA UN PRODUCTO EN EL ARREGLO Productos Y REGRESA EL INDICE DONDE SE ALMACENA SU INFORMACION
int VerifyCode(wstring cod);								//FUNCION QUE VALIDA EL CODIGO DE PRODUCTO INGRESADO POR EL USUARIO
int VerifyCodeForEdit(wstring cod);							//FUNCION QUE VALIDA EL CODIGO DE PRODUCTO INGRESADO POR EL USUARIO EN UNA EDICION DE PRODUCTO
int VerifyCodeForSales(wstring cod);						//FUNCION QUE VALIDA EL CODIGO DE PRODUCTO INGRESADO POR EL USUARIO EN UNA VENTA
int VerifyName(wstring Name);								//FUNCION QUE VALIDA EL NOMBRE DE PRODUCTO INGRESADO POR EL USUARIO
int GetCant();												//FUNCION QUE OBTIENE Y VALIDA LA CANTIDAD DE PRODUCTOS INRESADA POR EL USUARIO
bool AnyIsNotDigit(const wstring& cod_punt);				//FUNCION QUE REVISA SI ALGUNO DE LOS CARACTERES DE UN STRING NO ES DIGITO
bool AnyIsNotAlphanum(const wstring s_punt);				//FUNCION QUE REVISA SI ALGUNO DE LOS CARACTERES DE UN STRING NO ES DIGITO
bool LookForCoincidence(wstring clave);						//FUNCION QUE REVISA SI UN CODIGO DE PRODUCTO YA EXISTE DENTRO DEL ARREGLO Productos
float GetPrice();											//FUNCION QUE OBTIENE Y VALIDA EL PRECIO UNITARIO INGRESADO POR EL USUARIO
char GetResp();												//FUNCION PARA TOMAR LA RESPUESTA DEL USUARIO, VERIFICARLA Y MARCAR POSIBLES ERRORES
void ShowMainMenu();										//FUNCION QUE IMPRIME EL MENU PRINCIPAL
void ShowSalesMenu();										//FUNCION QUE IMPRIME EL MENU DE VENTAS
void ShowPurchasesMenu();									//FUNCION QUE IMPRIME EL MENU DE COMPRAS
void ShowProductListOptions();								//FUNCION QUE MUESTRA LAS OPCIONES DE EDICION DE LA LISTA DE PRECIOS
void EditProductOptions();									//FUNCION QUE MUESTRA LAS OPCIONES DE EDICION DE UN PRODUCTO
void EditProduct(const int indice, const int caso);			//FUNCION QUE EDITA LA INFORMACION DE UN PRODUCTO
void EraseProduct();										//FUNCION QUE ELIMINA UN PRODUCTO EXISTENTE
void MakeASale();											//FUNCION QUE INICIA EL PROCESO PARA HACER UNA VENTA
void ShowCurrentSales();									//FUNCION QUE MUESTRA LAS VENTAS DEL DIA
void SignUpAProduct();										//FUNCION QUE INICIA EL PROCESO PARA DAR DE ALTA UN PRODUCTO
void SignUpAgain();											//FUNCION QUE PREGUNTA AL USUARIO SI QUIERE VOLVER A REGISTRAR UN PRODUCTO O NO
void MakeAnotherSale();										//FUNCION QUE PREGUNTA AL USUARIO SI QUIERE VOLVER A HACER UNA VENTA O NO
void FillRegister(int i);									//FUNCION COMPLEMENTARIA DE SignUpAProduct. SIRVE PARA TERMINAR EL PROCESO DE ALTA DE PRODUCTOS
void ShowProductsList();									//FUNCION QUE MUESTRA LA LISTA DE PRODUCTOS INGRESADOS
void BubbleSort(Product *productos, const int elementos);	//FUNCION QUE ORDENA UN ARREGLO DE PRODUCTOS A PARTIR DE SU CODIGO
void strtoupp(wstring *s);									//FUNCION QUE CONVIERTE UN wstring A MAYUSCULAS
void ToExit();												//FUNCION QUE PARA SALIR DEL SISTEMA
void WriteFile();											//FUNCION QUE GUARDA LOS PRODUCTOS REGISTRADOS EN UN ARCHIVO TXT
void FillingProducts();										//FUNCION PARA LLENAR EL ARREGLO PRODUCTOS

//------------------- FUNCIONES PARA LA IMPRESION Y NAVEGACION DE MENUS -------------------//

void main() {

	setlocale(LC_ALL, "");

	system("color F0");

	//MENSAJE DE BIENVENIDA

	cout << "BIENVENIDO AL SISTEMA DE LA FERRETERÍA TUER-K" << endl << endl;
	system("pause");

	FillingProducts();

	ShowMainMenu();

}

char GetResp() {

	//ESTA FUNCION SIRVE PARA TOMAR LA RESPUESTA DEL USUARIO, VERIFICARLA Y MARCAR POSIBLES ERRORES

	char resp = '0';

	do {

		cin >> setw(1) >> resp;

		if (cin.fail()) {

			//IMPRESION DEL MENSAJE DE ERROR
			cout << endl << "Error al introducir la respuesta, intente de nuevo" << endl;

			//CLEAR STREAM
			cin.clear();
			cin.ignore(INT_MAX, '\n');

		}

	} while (cin.fail());

	//CLEAR STREAM
	cin.clear();
	cin.ignore(INT_MAX, '\n');

	return resp;

}

void ShowMainMenu() {

	system("cls");

	char resp = '0';

	//IMPRESIÓN DEL MENÚ PRINCIPAL

	cout << "Menú" << endl << endl <<
		"Seleccione una opción ingresando el número correspondiente:" << endl << endl <<
		"1) Ventas" << endl <<
		"2) Compras" << endl <<
		"3) Salir" << endl;
	resp = GetResp(); //SE LLAMA A LA FUNCION GetResp PARA LEER Y VALIDAR LA RESPUESTA

	//SWITCH QUE INTERPRETA LA RESPUESTA DEL USUARIO

	switch (resp) {
	case '1':
		ShowSalesMenu();
		break;
	case '2':
		ShowPurchasesMenu();
		break;
	case '3':
		ToExit();
		break;
	default:
		cout << "Opción Inválida" << endl;
		system("pause");
		ShowMainMenu();
	}

}

void ShowSalesMenu() {

	system("cls");

	char resp = '0';

	//IMPRESIÓN DEL MENÚ DE VENTAS

	cout << "Ventas" << endl << endl <<
		"Seleccione una opción" << endl << endl <<
		"1) Realizar una venta" << endl <<
		"2) Ver ventas del día" << endl <<
		"3) Volver" << endl;
	resp = GetResp(); //SE LLAMA A LA FUNCION GetResp PARA LEER Y VALIDAR LA RESPUESTA

	//SWITCH QUE INTERPRETA LA RESPUESTA DEL USUARIO

	switch (resp) {
	case '1':
		MakeASale();
		break;
	case '2':
		ShowCurrentSales();
		break;
	case '3':
		ShowMainMenu();
		break;
	default:
		cout << "Opción Inválida" << endl;
		system("pause");
		ShowSalesMenu();
	}

}

void ShowPurchasesMenu() {

	system("cls");

	char resp = '0';

	//IMPRESIÓN DEL MENÚ DE COMPRAS

	cout << "Compras" << endl << endl <<
		"Seleccione una opción" << endl << endl <<
		"1) Dar un producto de alta" << endl <<
		"2) Ver lista de productos" << endl <<
		"3) Volver" << endl;
	resp = GetResp(); //SE LLAMA A LA FUNCION GetResp PARA LEER Y VALIDAR LA RESPUESTA

	//SWITCH QUE LEE LA RESPUESTA DEL USUARIO

	switch (resp) {
	case '1':
		SignUpAProduct();
		break;
	case '2':
		ShowProductsList();
		break;
	case '3':
		ShowMainMenu();
		break;
	default:
		cout << "Opción Inválida" << endl;
		system("pause");
		ShowPurchasesMenu();
	}

}

void ToExit() {

	system("cls");
	cout << "Está a punto de salir del sistema, ¿Quiere continuar?" << endl <<
		"1) No, volver" << endl <<
		"2) Sí, cerrar el programa" << endl;

	char resp = '0';
	resp = GetResp();

	switch (resp) {

	case '1':
		ShowMainMenu();
		break;
	case '2':
		WriteFile();
		exit(0);
		break;
	default:
		cout << "Opción inválida, intente de nuevo" << endl;
		system("pause");
		ToExit();

	}

}

//------------------- FUNCIONES PARA EL PROCESO Y LISTA DE VENTAS -------------------//

void MakeASale() {

	int band;
	wstring cod_aux;

	system("cls");

	//IMPRESIÓN DEL MENÚ PARA REALIZAR UNA COMPRA

	cout << "Realizar una venta" << endl << endl <<
		"Ingrese el código del producto a vender o Enter para volver" << endl;
	getline(wcin, cod_aux);

	band = VerifyCodeForSales(cod_aux);

	switch (band) {

	case 1:
	case 4:
		system("pause");
		MakeASale();
		break;
	case 2:
	case 3:
	case 5:
		system("pause");
		ShowSalesMenu();
		break;

	}

	int cant_aux, indice_venta, indice_prod;

	if (band == 0) {

		//ASIGNACION DE VALORES A LOS INDICES: INDICE PARA EL ARREGLO VENTAS E INDICE DEL PRODUCTO BUSCADO
		indice_venta = LookForSalesSpace();
		indice_prod = SearchProduct(cod_aux);

		//SE PIDE LA CANTIDAD A VENDER
		do {

			system("cls");

			cout << "Ingrese la cantidad a vender: ";
			cant_aux = GetCant();

			if (cant_aux > Productos[indice_prod].cant) {

				if (Productos[indice_prod].cant != 0) {

					cout << "La cantidad que introdujo es mayor a la cantidad de productos disponibles." << endl;
					system("pause");

				}
				else {

					cout << "El producto está agotado." << endl;
					system("pause");
					ShowSalesMenu();

				}

			}

		} while (cant_aux == -1 || cant_aux > Productos[indice_prod].cant);

		//LLENADO Y ACTUALIZACION DE LA INFORMACION

		Ventas[indice_venta].cod_prod = cod_aux;								//CODIGO DEL PRODUCTO VENDIDO
		Ventas[indice_venta].product = Productos[indice_prod].name;				//NOMBRE DEL PRODUCTO VENDIDO
		Ventas[indice_venta].cant = cant_aux;									//CANTIDAD DE UNIDADES VENDIDAS
		Ventas[indice_venta].price_x_unit = Productos[indice_prod].price;		//PRECIO POR UNIDAD DE PRODUCTO
		Ventas[indice_venta].total = Productos[indice_prod].price * cant_aux;	//TOTAL A PAGAR POR LA VENTA

		Productos[indice_prod].cant -= cant_aux; //SE ACTUALIZA LA CANTIDAD DE UNIDADES DISPONIBLES

		//PREGUNTA AL USUARIO SI QUIERE REALIZAR OTRA VENTA

		MakeAnotherSale();

	}

}

void MakeAnotherSale() {

	//FUNCION QUE PREGUNTA AL USUARIO SI QUIERE VOLVER A HACER UNA VENTA O NO

	system("cls");

	char resp = '0';

	cout << "Venta realizada con éxito." << endl << "¿Quiere realizar otra venta?" << endl << endl <<
		"1) Sí, realizar otra venta" << endl << "2) No, volver al menú de ventas" << endl;

	resp = GetResp();

	switch (resp) {

	case '1':
		MakeASale();
		break;
	case '2':
		ShowSalesMenu();
		break;
	default:
		cout << "Opción inválida. Intente de nuevo." << endl;
		system("pause");
		MakeAnotherSale();

	}

}

void ShowCurrentSales() {

	system("cls");

	cout << fixed;
	cout << setprecision(2);

	cout << "Ventas del día" << endl << endl;
	cout << left << setw(10) << "No. Venta";
	cout << left << setw(10) << "Código";
	cout << left << setw(37) << "Producto";
	cout << right << setw(18) << "Precio unitario";
	cout << right << setw(10) << "Cantidad";
	cout << right << setw(10) << "Total";
	cout << endl << endl;

	int i = 0;
	float subtotal = 0;

	if (LookForSalesSpace() > 0 || Ventas[199].cod_prod != L"") {

		while (i < 200) {

			if (Ventas[i].cod_prod != L"") {

				cout << left << setw(10) << i + 1;
				wcout << left << setw(10) << Ventas[i].cod_prod;
				wcout << left << setw(37) << Ventas[i].product;
				cout << right << setw(18) << Ventas[i].price_x_unit;
				cout << right << setw(10) << Ventas[i].cant;
				cout << right << setw(10) << Ventas[i].total << endl;

				subtotal += Ventas[i].total;

			}
			else
				i = 199; //SE ROMPE EL CICLO

			i++;

		}

		cout << endl;
		cout << right << setw(85) << "Subtotal:" << right << setw(10) << subtotal << endl;
		cout << right << setw(85) << "IVA:" << right << setw(10) << subtotal * 0.16 << endl;
		cout << right << setw(85) << "Total final:" << right << setw(10) << subtotal * 1.16 << endl;

	}
	else {

		cout << "No se han realizado ventas." << endl;

	}

	system("pause");
	ShowSalesMenu();

}

int VerifyCodeForSales(wstring cod) {

	int band = 0;

	if (!cod.empty()) { //LA CONDICION EVALUA SI EL CODIGO NO ESTA VACIO

		if (LookForSpace() > 0) { //LA CONDICION EVALUA SI HAY AL MENOS UN PRODUCTO REGISTRADO

			if (LookForSalesSpace() >= 0) { //LA CONDICION EVALUA SI HAY ESPACIO EN EL ARREGLO Ventas

				if (AnyIsNotDigit(cod)) { //LA CONDICION LLAMA LA FUNCION AnyIsNotDigit QUE EVALUA SI EL CODIGO CONTIENE ALGUN CARACTER DISTINTO A UN DIGITO

					band = 1;
					cout << endl << "El código de producto sólo puede tener números. Vuelva a intentarlo." << endl;

				}
				else {

					if (cod.length() != 8) { //LA CONDICION EVALUA SI EL CODIGO TIENE MAS DE 8 CARACTERES

						band = 1;
						cout << endl << "El código de producto debe tener 8 caracteres. Vuelva a intentarlo." << endl;

					}
					else {

						if (!LookForCoincidence(cod)) { //LA CONDICION LLAMA A LA FUNCION LookForCoincidence PARA EVALUAR SI EL CODIGO YA HA SIDO REGISTRADO

							band = 4; //SI EL PRODUCTO NO SE ENCUENTRA, band=4
							cout << endl << "El producto que intenta vender no existe." << endl;

						}

					}
				}

			}
			else {

				band = 5; //SI SE HA ALCANZADO EL LIMITE DE VENTAS, band=5
				cout << endl << "Se ha alcanzado el limite de ventas." << endl;

			}

		}
		else {

			band = 3; //SI NO HAY AL MENOS UN PRODUCTO REGISTRADO, band=3
			cout << endl << "No es posible hacer un venta, ya que no hay productos registrados." << endl;
		}

	}
	else band = 2; //SI EL STRING ESTÁ VACIO, band=2

	//CLEAR STREAM
	fflush(stdin);

	return band;
	//band=0 INDICA QUE EL CODIGO INTRODUCIDO ES CORRECTO
	//band=1 INDICA QUE EL CODIGO ES INCORRECTO
	//band=2 INDICA QUE EL CODIGO INTRODUCIDO ESTA VACIO
	//band=3 INDICA QUE NO HAY PRODUCTOS REGISTRADOS
	//band=4 INDICA QUE EL PRODUCTO NO EXISTE
	//band=5 INDICA QUE NO HAY ESPACIO EN EL ARREGLO Ventas

}

int LookForSalesSpace() {

	int band = 0, i = 0;

	while (band == 0 && i < 100) {

		if (Ventas[i].cod_prod == L"")
			band = 1;
		else
			i++;

	}

	if (band != 1)
		i = -1;

	return i;

}

//------------------- FUNCIONES PARA EL REGISTRO Y LISTA DE PRODUCTOS -------------------//

void SignUpAProduct() {

	/*ESTA FUNCION MUESTRA LA PANTALLA "Dar de alta un producto", ADEMAS PIDE AL USUARIO INGRESAR UN CODIGO DE PRODUCTO.
	EL CODIGO INTRODUCIDO SE GUARDA EN UNA VARIABLE AUXILIAR QUE SE VERIFICA USANDO LA FUNCION VerifyCode, LA CUAL
	DEVUELVE UN NUMERO DE ERROR Y ESTA FUNCION ACTUA DE ACUERDO AL NUMERO DE ERROR. SI NO HAY ERROR EN EL CODIGO, SE
	LLAMA A LA FUNCION LookForSpace QUE REGRESA UN VALOR IGUAL AL PRIMER INDICE LIBRE DEL ARREGLO Productos. SE ASIGNA
	EL VALOR DE cod_aux AL CODIGO DEL REGISTRO DEL ARREGLO Productos CON EL INDICE DEVUELTO POR LookForSpace. POSTERIORMENTE
	SE LLAMA A LA FUNCION FillRegister QUE TOMA COMO ENTRADA EL VALOR DEL INDICE DEVUELTO POR LookForSpace (PARA MAS
	INFORMACION DE LO QUE HACE LA FUNCION FillRegister, CONSULTAR EL CODIGO DE ESTA).*/

	system("cls");

	int band, indice;
	wstring cod_aux; //cod_aux ES UNA VARIABLE AUXILIAR PARA GUARDAR EL CODIGO DE PRODUCTO

	cout << "Dar de alta un producto" << endl << endl <<
		"Cree un código de producto de 8 dígitos o presione Enter para volver" << endl;
	getline(wcin, cod_aux);

	band = VerifyCode(cod_aux);

	//switch PARA band QUE PERMITE RECURSIVIDAD (CICLADO) DEPENDIENDO DEL NUMERO DE ERROR ARROJADO POR VerifyCode

	switch (band) {

	case 1:
	case 4:
		system("pause");
		SignUpAProduct();
		break;
	case 2:
	case 3:
		system("pause");
		ShowPurchasesMenu();
		break;

	}

	if (band == 0) { //EL SIGUIENTE PROCESO SOLO SE REALIZA SI LA INFORMACION QUE SE INTRODUJO ES CORRECTA

		indice = LookForSpace();

		//TRAS ASEGURARSE QUE TODO ESTA BIEN CON cod_aux, SE ASIGNA cod_aux AL CODIGO DEL PRIMER REGISTRO LIBRE DE Productos

		Productos[indice].cod = cod_aux;

		FillRegister(indice); //LA FUNCION FillRegister TERMINA DE LLENAR EL REGISTRO

		//MENSAJE PARA VOLVER A REGISTRAR UN PRODUCTO

		SignUpAgain();

	}
}

void SignUpAgain() {

	system("cls");

	char resp = '0';

	cout << "Producto registrado." << endl << "¿Quiere dar de alta otro producto?" << endl << endl <<
		"1) Sí, dar de alta otro producto" << endl << "2) No, volver al menú de compras" << endl;

	resp = GetResp();

	switch (resp) {

	case '1':
		SignUpAProduct();
		break;
	case '2':
		ShowPurchasesMenu();
		break;
	default:
		cout << "Opción inválida. Intente de nuevo" << endl;
		system("pause");
		SignUpAgain();
	}

}

void FillRegister(int i) {

	//LAS SIGUIENTES (name_aux, price_aux y cant_aux) SON VARIABLES AUXILIARES QUE SE VERIFICARAN ANTES DE ASIGNARSE AL REGISTRO

	wstring name_aux;
	float price_aux;
	int cant_aux, band;

	do {

		do {

			do {

				system("cls");

				wcout << "Llenado de información para el producto " << Productos[i].cod << endl << endl <<
					"Ingrese el nombre del producto:" << endl;
				getline(wcin, name_aux);
				band = VerifyName(name_aux);

			} while (band != 0);

			strtoupp(&name_aux); //ESTA SENTENCIA LLAMA A LA FUNCION strtoupp QUE CAMBIA EL string a MAYUSCULAS

			cout << "Ingrese el precio unitario del producto: ";
			price_aux = GetPrice();

		} while (price_aux == -1); //price_aux = -1 ES UN POSIBLE RESULTADO ARROJADO POR GetPrice. SIRVE PARA INDICAR ERROR DE LECTURA

		cout << "Ingrese la cantidad de unidades disponibles: ";
		cant_aux = GetCant();

	} while (cant_aux == -1); //cant_aux = -1 ES UN POSIBLE RESULTADO ARROJADO POR GetCant. SIRVE PARA INDICAR ERROR DE LECTURA

	//TRAS HABERSE VALIDADO LA INFORMACION INGRESADA POR EL USUARIO, ESTA SE ASIGNA AL REGISTRO i

	Productos[i].name = name_aux;
	Productos[i].price = price_aux;
	Productos[i].cant = cant_aux;

	BubbleSort(Productos, 100);

}

float GetPrice() {

	float price;

	cin >> setw(4) >> price;

	if (cin.fail()) {

		//IMPRESION DEL MENSAJE DE ERROR
		cout << "Error al introducir la respuesta, intente de nuevo" << endl;

		system("pause");

		price = -1; //price = -1 ES UN INDICADOR DE ERROR, LO QUE PERMITE CICLAR EL PROCEDIMIENTO EN CASO DE ERROR.

	}
	else {

		if (price <= 0) {

			cout << "El precio no puede ser negativo ni cero. Intente de nuevo." << endl;

			system("pause");

			price = -1; //price = -1 ES UN INDICADOR DE ERROR, LO QUE PERMITE CICLAR EL PROCEDIMIENTO EN CASO DE ERROR.

		}
		else {

			if (price > 99999) {

				cout << "El precio no puede ser mayor a 99999. Intente de nuevo." << endl;

				system("pause");

				price = -1; //price = -1 ES UN INDICADOR DE ERROR, LO QUE PERMITE CICLAR EL PROCEDIMIENTO EN CASO DE ERROR.

			}

		}

	}

	//CLEAR STREAM
	cin.clear();
	cin.ignore(INT_MAX, '\n');

	return price;

}

int GetCant() {

	int cant;

	cin >> setw(5) >> cant;

	if (cin.fail()) {

		//IMPRESION DEL MENSAJE DE ERROR
		cout << "Error al introducir la respuesta, intente de nuevo" << endl;

		system("pause");

		cant = -1; //cant = -1 ES UN INDICADOR DE ERROR, LO QUE PERMITE CICLAR EL PROCEDIMIENTO EN CASO DE ERROR.

	}
	else {

		if (cant <= 0) {

			cout << "La cantidad de unidades no puede ser negativa ni cero. Intente de nuevo." << endl;

			system("pause");

			cant = -1; //cant = -1 ES UN INDICADOR DE ERROR, LO QUE PERMITE CICLAR EL PROCEDIMIENTO EN CASO DE ERROR.

		}
		else {

			if (cant > 999) {

				cout << "La cantidad de unidades no puede mayor a 999. Intente de nuevo." << endl;

				system("pause");

				cant = -1; //cant = -1 ES UN INDICADOR DE ERROR, LO QUE PERMITE CICLAR EL PROCEDIMIENTO EN CASO DE ERROR.

			}

		}

	}

	//CLEAR STREAM
	cin.clear();
	cin.ignore(INT_MAX, '\n');

	return cant;

}

void ShowProductsList() {

	system("cls");

	cout << fixed;
	cout << setprecision(2);

	cout << "Lista de productos" << endl << endl;
	cout << left << setw(10) << "Código";
	cout << left << setw(37) << "Nombre";
	cout << right << setw(10) << "Precio";
	cout << right << setw(10) << "Cantidad";
	cout << endl << endl;

	int i = 0;

	if (LookForSpace() > 0 || Productos[99].cod != L"") { //LA CONDICION EVALUA SI HAY PRODUCTOS REGISTRADOS DE INDICES 0-99

		while (i < 100) {

			if (Productos[i].cod != L"") {

				wcout << left << setw(10) << Productos[i].cod;
				wcout << left << setw(37) << Productos[i].name;
				cout << right << setw(10) << Productos[i].price;
				cout << right << setw(10) << Productos[i].cant << endl;

			}
			else
				i = 99; //SE ROMPE EL CICLO

			i++;

		}

		ShowProductListOptions(); //LAS OPCIONES SOLO SE MUESTRAN SI HAY PRODUCTOS REGISTRADOS

	}
	else {

		cout << "No hay productos registrados." << endl;
		system("pause");
		ShowPurchasesMenu();

	}

}

void ShowProductListOptions() {

	cout << endl << "1) Editar información de producto" << endl <<
		"2) Borrar producto" << endl <<
		"3) Volver al menú de compras" << endl;

	char resp = '0';

	resp = GetResp();

	switch (resp) {

	case '1':
		EditProductOptions();
		break;
	case '2':
		EraseProduct();
		break;
	case '3':
		ShowPurchasesMenu();
		break;
	default:
		cout << "Opción inválida, intente de nuevo." << endl;
		system("pause");
		ShowProductsList();

	}

}

void EditProductOptions() {

	system("cls");

	cout << "Seleccionó la opción de editar producto, ¿quiere continuar?" << endl <<
		"1) Sí" << endl << "2) No" << endl;

	char resp = '0', resp2 = '0';
	int band, band2, indice;
	wstring cod_aux;

	resp = GetResp();

	switch (resp) {

	case '1':

		do {

			system("cls");

			cout << "Ingrese el código del producto que desea editar:" << endl;
			getline(wcin, cod_aux);

			band = VerifyCodeForEdit(cod_aux);

			if (band == 3)
				ShowProductsList();

		} while (band != 0);

		indice = SearchProduct(cod_aux); //SE BUSCA EL INDICE DEL PRODUCTO SELECCIONADO

		do {

			system("cls");

			band2 = 0;

			cout << endl << "Seleccione la información que quiere editar:" << endl <<
				"1) Nombre del producto" << endl <<
				"2) Precio unitario" << endl <<
				"3) Cantidad de unidades disponibles" << endl;
			resp2 = GetResp();

			switch (resp2) {

			case '1':
				EditProduct(indice, 1);
				break;
			case '2':
				EditProduct(indice, 2);
				break;
			case '3':
				EditProduct(indice, 3);
				break;
			default:
				band2 = 1;
				cout << "Opción inválida. Intente de nuevo." << endl;
				system("pause");

			}

		} while (band2 != 0);

		break;
	case '2':
		ShowProductsList();
		break;
	default:
		cout << "Opción inválida. Intente de nuevo." << endl;
		system("pause");
		EditProductOptions();

	}

}

void EditProduct(const int indice, const int caso) {

	wstring name_aux;
	float price_aux;
	int band, cant_aux;

	switch (caso) {

	case 1: //EDITAR NOMBRE

		do {

			system("cls");

			cout << "Ingrese el nuevo nombre del producto" << endl;
			getline(wcin, name_aux);
			band = VerifyName(name_aux);

		} while (band != 0);

		strtoupp(&name_aux); //ESTA SENTENCIA LLAMA A LA FUNCION strtoupp QUE CAMBIA EL string a MAYUSCULAS

		Productos[indice].name = name_aux;

		break;
	case 2: //EDITAR PRECIO
		do {

			system("cls");

			cout << "Ingrese el nuevo precio unitario del producto: ";
			price_aux = GetPrice();

		} while (price_aux == -1); //price_aux = -1 ES UN POSIBLE RESULTADO ARROJADO POR GetPrice. SIRVE PARA INDICAR ERROR DE LECTURA

		Productos[indice].price = price_aux;

		break;
	case 3: //EDITAR CANTIDAD

		do {

			system("cls");

			cout << "Ingrese la nueva cantidad de unidades disponibles: ";
			cant_aux = GetCant();

		} while (cant_aux == -1); //cant_aux = -1 ES UN POSIBLE RESULTADO ARROJADO POR GetCant. SIRVE PARA INDICAR ERROR DE LECTURA

		Productos[indice].cant = cant_aux;

		break;

	}

	cout << "Producto editado con éxito." << endl;
	system("pause");
	ShowProductsList();

}

void EraseProduct() {

	system("cls");

	cout << "Seleccionó la opción de borrar producto, ¿quiere continuar?" << endl <<
		"1) Sí" << endl << "2) No" << endl;

	char resp = '0';
	int band, indice, recorrido;
	wstring cod_aux;

	resp = GetResp();

	if (resp == '1') {

		do {

			system("cls");

			cout << "Ingrese el código del producto que desea borrar:" << endl;
			getline(wcin, cod_aux);

			band = VerifyCodeForEdit(cod_aux);

			if (band == 3)
				ShowProductsList();

		} while (band != 0);

		indice = SearchProduct(cod_aux); //SE BUSCA EL INDICE DEL PRODUCTO SELECCIONADO
		recorrido = indice; //SE INICIALIZA LA VARIBLE recorrido EN EL INDICE DEL PRODUCTO SELECCIONADO

		//PROCESO DE ELIMINACIÓN Y REORDENACIÓN
		while (recorrido < 100) {

			if (Productos[recorrido].cod != L"") {

				if (recorrido != 99) //EVALUA SI EL PRODUCTO A BORRAR ES DISTINTO DEL ULTIMO ELEMENTO DEL ARREGLO Productos
					Productos[recorrido] = Productos[recorrido + 1];
				else { //SI SE INTENTA BORRAR EL ULTIMO PRODUCTO DEL ARREGLO Productos, BORRAMOS SU INFORMACION

					Productos[recorrido].cod = L"";
					Productos[recorrido].name = L"";
					Productos[recorrido].price = 0;
					Productos[recorrido].cant = 0;

				}
			}
			else
				recorrido = 99;

			recorrido++;

		}

		cout << "Se borró el producto exitosamente." << endl;

	}

	if (resp == '1' || resp == '2') { //SE COMPRUEBA QUE SEA UNA OPCION CORRECTA

		system("pause");
		ShowProductsList();

	}
	else {

		cout << "Opción inválida. Intente de nuevo." << endl;
		system("pause");
		EraseProduct();

	}

}

int VerifyCode(wstring cod) {

	int band = 0;

	if (!cod.empty()) { //LA CONDICION EVALUA SI EL CODIGO NO ESTA VACIO

		if (LookForSpace() >= 0) { //LA CONDICION EVALUA SI HAY ESPACIO PARA GUARDAR UN NUEVO PRODUCTO

			if (AnyIsNotDigit(cod)) { //LA CONDICION LLAMA LA FUNCION AnyIsNotDigit QUE EVALUA SI EL CODIGO CONTIENE ALGUN CARACTER DISTINTO A UN DIGITO

				band = 1;
				cout << endl << "El código de producto sólo puede tener números. Vuelva a intentarlo." << endl;

			}
			else {

				if (cod.length() != 8) { //LA CONDICION EVALUA SI EL CODIGO TIENE MAS DE 8 CARACTERES

					band = 1;
					cout << endl << "El código de producto debe tener 8 caracteres. Vuelva a intentarlo." << endl;

				}
				else {

					if (LookForCoincidence(cod)) { //LA CONDICION LLAMA A LA FUNCION LookForCoincidence PARA EVALUAR SI EL CODIGO YA HA SIDO REGISTRADO

						band = 4; //SI YA HAY UN REGISTRO IGUAL, band=4
						cout << endl << "El código que intenta ingresar ya ha sido registrado." << endl;

					}

				}
			}
		}
		else {

			band = 3; //SI NO HAY ESPACIO PARA GUARDAR UN NUEVO REGISTRO, band=3
			cout << endl << "No es posible dar de alta un nuevo producto porque se ha alcanzado el máximo de productos." << endl;
		}
	}
	else band = 2; //SI EL STRING ESTÁ VACIO, band=2

	//CLEAR STREAM
	fflush(stdin);

	return band;
	//band=0 INDICA QUE EL CODIGO INTRODUCIDO ES CORRECTO
	//band=1 INDICA QUE EL CODIGO ES INCORRECTO
	//band=2 INDICA QUE EL CODIGO INTRODUCIDO ESTA VACIO
	//band=3 INDICA QUE NO HAY ESPACIO PARA GUARDAR UN NUEVO PRODUCTO
	//band=4 INDICA QUE EL PRODUCTO YA HA SIDO REGISTRADO

}

int VerifyCodeForEdit(wstring cod) {

	int band = 0;

	if (!cod.empty()) { //LA CONDICION EVALUA SI EL CODIGO NO ESTA VACIO

		if (AnyIsNotDigit(cod)) { //LA CONDICION LLAMA LA FUNCION AnyIsNotDigit QUE EVALUA SI EL CODIGO CONTIENE ALGUN CARACTER DISTINTO A UN DIGITO

			band = 1;
			cout << endl << "El código de producto sólo puede tener números. Vuelva a intentarlo." << endl;
			system("pause");

		}
		else {

			if (cod.length() != 8) { //LA CONDICION EVALUA SI EL CODIGO TIENE MAS DE 8 CARACTERES

				band = 1;
				cout << endl << "El código de producto debe tener 8 caracteres. Vuelva a intentarlo." << endl;
				system("pause");

			}
			else {

				if (!LookForCoincidence(cod)) { //LA CONDICION LLAMA A LA FUNCION LookForCoincidence PARA EVALUAR SI EL CODIGO YA HA SIDO REGISTRADO

					band = 3; //SI EL PRODUCTO NO SE ENCUENTRA, band=3
					cout << endl << "El producto seleccionado no existe." << endl;
					system("pause");

				}

			}
		}

	}
	else {

		band = 2; //SI EL STRING ESTÁ VACIO, band=2
		cout << endl << "No se introdujo un código." << endl;
		system("pause");

	}

	//CLEAR STREAM
	fflush(stdin);

	return band;
	//band=0 INDICA QUE EL CODIGO INTRODUCIDO ES CORRECTO
	//band=1 INDICA QUE EL CODIGO ES INCORRECTO
	//band=2 INDICA QUE EL CODIGO INTRODUCIDO ESTA VACIO
	//band=3 INDICA QUE EL PRODUCTO NO EXISTE

}

int VerifyName(wstring Name) {

	int band = 0;

	if (!Name.empty()) {

		if (Name.length() > 35) {

			band = 1;
			cout << endl << "El nombre del producto no puede tener más de 35 caracteres. Vuelva a intentarlo." << endl;
			system("pause");

		}
		else {

			if (AnyIsNotAlphanum(Name)) {

				band = 3;
				cout << endl << "Ingrese el nombre sin acentos ni ñ." << endl;
				system("pause");

			}

		}

	}
	else {

		band = 2;
		cout << endl << "No se ha introducido el nombre, intente de nuevo." << endl;
		system("pause");

	}

	fflush(stdin);

	return band;
	//band=1 INDICA QUE EL NOMBRE SUPERA LOS 35 CARACTERES
	//band=2 INDICA QUE EL NOMBRE NO SE HA INTRODUCIDO
	//band=3 INDICA QUE SE HA INTENTADO INTRODUCIR UN NOMBRE CON ACENTOS O Ñ

}

void strtoupp(wstring *s) {

	for (unsigned int i = 0; i < (*s).length(); i++) {

		(*s)[i] = towupper((*s)[i]);

	}

}

//------------------- FUNCIONES DE FICHEROS -------------------//

void WriteFile() {

	//FUNCION QUE ESCRIBE UN ARCHIVO DE TEXTO CON LA LISTA DE PRODUCTOS EXISTENTES

	system("cls");

	char resp = '0';

	if (LookForSpace() > 0 || Productos[99].cod != L"") {

		cout << "¿Quiere guardar un archivo de texto con la lista de productos registrados?" << endl <<
			"1) Sí, guardar archivo" << endl <<
			"2) No, salir sin guardar" << endl;
		resp = GetResp();

		wofstream lista; //CREACION DEL FLUJO A FICHERO

		switch (resp) {

		case '1':
			//SE ABRE/CREA EL ARCHIVO Y SE ESCRIBE LA INFORMACION EN EL
			lista.open("Lista_de_productos.txt");

			if (lista) { //SE REVISA QUE EL ARCHIVO SE HAYA ABIERTO CORRECTAMENTE

				//IMPRESION DE ENCABEZADO
				lista << fixed;
				lista << setprecision(2);

				lista << "Lista de productos" << endl << endl;
				lista << left << setw(10) << "|Código|";
				lista << left << setw(37) << "|Nombre|";
				lista << right << setw(10) << "|Precio|";
				lista << right << setw(10) << "|Cantidad|";
				lista << endl << endl;

				//IMPRESION DE LA LISTA

				int i = 0;

				while (i < 100) {

					if (Productos[i].cod != L"") {

						lista << left << setw(10) << Productos[i].cod;
						lista << left << setw(37) << Productos[i].name;
						lista << right << setw(10) << Productos[i].price;
						lista << right << setw(10) << Productos[i].cant << endl;

					}
					else
						i = 99; //SE ROMPE EL CICLO

					i++;

				}

			}
			else {

				cout << "El archivo no se pudo abrir" << endl;
				system("pause");

			}

			lista.close();

			break;
		case '2':
			break;
		default:
			cout << "Opción inválida, intente de nuevo" << endl;
			system("pause");
			WriteFile();

		}

	}

}

//------------------- FUNCIONES VARIAS -------------------//

int LookForSpace() {

	int band = 0, i = 0;

	while (band == 0 && i < 100) {

		if (Productos[i].cod == L"")
			band = 1;
		else
			i++;

	}

	if (band != 1) //BANDERA DE ERROR
		i = -1;

	return i;

}

int SearchProduct(wstring busq) {

	/*FUNCION QUE BUSCA UN DETERMINADO CODIGO EN EL ARREGLO Productos
	Y REGRESA EL INDICE DONDE SE ENCUENTRA ALMACENADA SU INFORMACION*/

	bool coincidence = false;
	int indice = 0;

	while (coincidence == false && indice < 100) {

		if (busq == Productos[indice].cod)
			coincidence = true;
		else
			indice++;

	}

	return indice;

}

bool LookForCoincidence(wstring clave) {

	bool coincidence = false;
	int i = 0;

	while (coincidence == false && i < 100) {

		if (clave == Productos[i].cod)
			coincidence = true;
		else
			i++;

	}

	return coincidence;
	//coincidence=false INDICA QUE NO HAY UN REGISTRO IGUAL
	//coincidence=true INDICA QUE SI HAY UN REGISTRO IGUAL

}

bool AnyIsNotDigit(const wstring& cod_punt) {

	//FUNCION QUE EVALUA SI EL STRING REFERENCIADO CONTIENE ALGUN CARACTER QUE NO ES UN DIGITO

	bool condicion;

	condicion = !all_of(cod_punt.begin(), cod_punt.end(), iswdigit);

	return condicion;

}

bool AnyIsNotAlphanum(const wstring s_punt) {

	//FUNCION QUE EVALUA SI EL STRING REFERENCIADO CONTIENE ALGUN CARACTER NO IMPRIMIBLE

	bool condicion = false;

	for (unsigned int i = 0; i < s_punt[i]; i++) {

		if (s_punt[i] < 32 || s_punt[i]>126)
			condicion = true;

	}

	return condicion;

}

void BubbleSort(Product *productos, const int elementos) {

	int i = 0, j = 0;

	Product aux;

	while (productos[i].cod != L"" && i < elementos - 1) {

		j = i + 1;

		while (productos[j].cod != L"" && j < elementos) {

			if (productos[i].cod > productos[j].cod) {

				aux = productos[i];
				productos[i] = productos[j];
				productos[j] = aux;

			}

			j++;

		}

		i++;

	}

}

void FillingProducts() {

	int cod = 10000000, cant1 = 20;
	float price = 5;
	wstring cod_aux, name;

	for (int i = 0; i < 20; i++) {

		cod_aux = to_wstring(cod + i);
		name = L"PRODUCTO " + to_wstring(i + 1);

		Productos[i].cod = cod_aux;
		Productos[i].name = name;
		Productos[i].price = price;
		Productos[i].cant = cant1;

	}

}
