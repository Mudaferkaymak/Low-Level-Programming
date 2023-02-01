#include <windows.h>
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <iostream>
#include "image_processing.cpp"

using namespace std;

void Dilation(int n, int filter_size, short* resimadres_org);
void Erosion(int n, int filter_size, short* resimadres_org);

int main(void) {
	int M, N, Q, i, j, filter_size;
	bool type;
	int efile;
	char org_resim[100], dil_resim[] = "dilated.pgm", ero_resim[] = "eroded.pgm";
	do {
		printf("Orijinal resmin yolunu (path) giriniz:\n-> ");
		scanf("%s", &org_resim);
		system("CLS");
		efile = readImageHeader(org_resim, N, M, Q, type);
	} while (efile > 1);
	int** resim_org = resimOku(org_resim);

	printf("Orjinal Resim Yolu: \t\t\t%s\n", org_resim);

	short *resimdizi_org = (short*) malloc(N*M * sizeof(short));

	for (i = 0; i < N; i++)
		for (j = 0; j < M; j++)
			resimdizi_org[i*N + j] = (short)resim_org[i][j];

	int menu;
	printf("Yapmak istediginiz islemi giriniz...\n");
	printf("1-) Dilation\n");
	printf("2-) Erosion\n");
	printf("3-) Cikis\n> ");
	scanf("%d", &menu);
	printf("Filtre boyutunu giriniz: ");
	scanf("%d", &filter_size);

	switch (menu){
		case 1:
			Dilation(N*M, filter_size, resimdizi_org);
			resimYaz(dil_resim, resimdizi_org, N, M, Q);
			break;
		case 2:
			Erosion(N*M, filter_size, resimdizi_org);
			resimYaz(ero_resim, resimdizi_org, N, M, Q);
			break;
		case 3:
			system("EXIT");
			break;
		default:
			system("EXIT");
			break;
	}

	system("PAUSE");
	return 0;
}

void Dilation(int n, int filter_size, short* resim_org) {
	short* resim_2 = (short*)malloc((n + 1) * sizeof(short));
	for (int i = 0; i < n; i++)
		resim_2[i] = resim_org[i];
	__asm {
		MOV EAX, filter_size
		CMP EAX, 3
		JE uc
		
		uc : MOV EDI, resim_2
		ADD EDI, 1026
		MOV ESI, resim_org
		ADD ESI, 1026
		MOV ECX, 510
		le1 : PUSH ECX
		MOV ECX, 510
		le2 : MOV EDX, [EDI]
		MOV EAX, [EDI - 1026]
		CMP EAX, EDX
		JGE d1
		MOV EDX, EAX
		d1 : MOV EAX, [EDI - 1024]
		CMP EAX, EDX
		JGE d2
		MOV EDX, EAX
		d2 : MOV EAX, [EDI - 1022]
		CMP EAX, EDX
		JGE d3
		MOV EDX, EAX
		d3 : MOV EAX, [EDI - 2]
		CMP EAX, EDX
		JGE d4
		MOV EDX, EAX
		d4 : MOV EAX, [EDI + 2]
		CMP EAX, EDX
		JGE d5
		MOV EDX, EAX
		d5 : MOV EAX, [EDI + 1022]
		CMP EAX, EDX
		JGE d6
		MOV EDX, EAX
		d6 : MOV EAX, [EDI + 1024]
		CMP EAX, EDX
		JGE d7
		MOV EDX, EAX
		d7 : MOV EAX, [EDI + 1026]
		CMP EAX, EDX
		JGE d8
		MOV EDX, EAX
		d8 : MOV[ESI], EDX
		ADD EDI, 2
		ADD ESI, 2
		LOOP le2
		POP ECX
		ADD EDI, 4
		ADD ESI, 4
		LOOP le1
	}
	free(resim_2);
	printf("\nDilation islemi sonucunda resim \"dilated.pgm\" ismiyle olusturuldu...\n");
}

void Erosion(int n, int filter_size, short* resim_org) {
	short* resim_2 = (short*)malloc((n + 1) * sizeof(short));
	for (int i = 0; i < n; i++)
		resim_2[i] = resim_org[i];
	__asm {

		MOV EAX, filter_size
		CMP EAX, 3
		JE uce

		uce : MOV EDI, resim_2
		ADD EDI, 1026
		MOV ESI, resim_org
		ADD ESI, 1026
		MOV ECX, 510
l1:		PUSH ECX
		MOV ECX, 510
    	l2 : MOV EDX, [EDI]
		MOV EAX, [EDI - 1026]
		CMP EAX, EDX
		JLE er1
		MOV EDX, EAX
		er1 : MOV EAX, [EDI - 1024]
		CMP EAX, EDX
		JLE er2
		MOV EDX, EAX
		er2 : MOV EAX, [EDI - 1022]
		CMP EAX, EDX
		JLE er3
		MOV EDX, EAX
		er3 : MOV EAX, [EDI - 2]
		CMP EAX, EDX
		JLE er4
		MOV EDX, EAX
		er4 : MOV EAX, [EDI + 2]
		CMP EAX, EDX
		JLE er5
		MOV EDX, EAX
		er5 : MOV EAX, [EDI + 1022]
		CMP EAX, EDX
		JLE er6
		MOV EDX, EAX
		er6 : MOV EAX, [EDI + 1024]
		CMP EAX, EDX
		JLE er7
		MOV EDX, EAX
		er7 : MOV EAX, [EDI + 1026]
		CMP EAX, EDX
		JLE er8
		MOV EDX, EAX
		er8 : MOV[ESI], EDX
		ADD EDI, 2
		ADD ESI, 2
		LOOP l2
		POP ECX
		ADD EDI, 4
		ADD ESI, 4
		LOOP l1
	}

	free(resim_2);
	printf("\nErosion islemi sonucunda resim \"eroded.pgm\" ismiyle olusturuldu...\n");
}
