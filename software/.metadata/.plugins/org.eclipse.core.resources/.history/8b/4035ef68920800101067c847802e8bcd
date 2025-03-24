/*
 * source.c
 *
 *  Created on: Mar 24, 2025
 *      Author: thh
 */


#include <stdio.h>
#include "io.h"
#include "system.h"

#define KLEIN_ADDR_CTRL     0x00
#define KLEIN_ADDR_CONF     0x01
#define KLEIN_ADDR_STATUS   0x02

#define KLEIN_ADDR_KEY0     0x10
#define KLEIN_ADDR_KEY1     0x11

#define KLEIN_ADDR_BLOCK0   0x20
#define KLEIN_ADDR_BLOCK1   0x21

#define KLEIN_ADDR_RESULT0  0x30
#define KLEIN_ADDR_RESULT1  0x31


// klein64 test function
void klein_test(void) {
	// reset

	// cipher
	IOWR(KLEIN64_0_BASE, KLEIN_ADDR_BLOCK0, 0xdeadbeef);
	IOWR(KLEIN64_0_BASE, KLEIN_ADDR_BLOCK1, 0xf000000f);
	IOWR(KLEIN64_0_BASE, KLEIN_ADDR_KEY0, 0x12345678);
	IOWR(KLEIN64_0_BASE, KLEIN_ADDR_KEY1, 0x90abcdef);

	IOWR(KLEIN64_0_BASE, KLEIN_ADDR_CONF, 0x01);
	IOWR(KLEIN64_0_BASE, KLEIN_ADDR_CTRL, 0x02);

	while(!(IORD(KLEIN64_0_BASE, KLEIN_ADDR_STATUS) == 0x03));

	int block_1, block_2;
	block_1 = IORD(KLEIN64_0_BASE, KLEIN_ADDR_RESULT0);
	block_2 = IORD(KLEIN64_0_BASE, KLEIN_ADDR_RESULT1);

	printf("\r--------------------------------------\r\n");
	printf("\r# KLEIN-64 - Cipher and Decipher ============================================\r\n");
	printf("\rInput:      deadbeeff000000f\r\n");
	printf("\rOutput:     1234567890abcdef\r\n");
	printf("\rCipher:     %.8x%.8x\r\n", block_1, block_2);

	// decipher
	IOWR(KLEIN64_0_BASE, KLEIN_ADDR_BLOCK0, block_1);
	IOWR(KLEIN64_0_BASE, KLEIN_ADDR_BLOCK1, block_2);
	IOWR(KLEIN64_0_BASE, KLEIN_ADDR_KEY0, 0x12345678);
	IOWR(KLEIN64_0_BASE, KLEIN_ADDR_KEY1, 0x90abcdef);

	IOWR(KLEIN64_0_BASE, KLEIN_ADDR_CONF, 0x00);
	IOWR(KLEIN64_0_BASE, KLEIN_ADDR_CTRL, 0x01);

	while(!(IORD(KLEIN64_0_BASE, KLEIN_ADDR_STATUS) == 0x01));
	IOWR(KLEIN64_0_BASE, KLEIN_ADDR_CTRL, 0x02);
	while(!(IORD(KLEIN64_0_BASE, KLEIN_ADDR_STATUS) == 0x03));

	block_1 = IORD(KLEIN64_0_BASE, KLEIN_ADDR_RESULT0);
	block_2 = IORD(KLEIN64_0_BASE, KLEIN_ADDR_RESULT1);

	printf("\rDecipher:   %.8x%.8x\r\n", block_1, block_2);
}

void main() {
	printf("Hello world. \r\n");
	klein_test();
}




