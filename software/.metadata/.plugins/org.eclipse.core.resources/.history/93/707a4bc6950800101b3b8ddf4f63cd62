/*
 * source.c
 *
 *  Created on: Mar 24, 2025
 *      Author: thh
 */


#include <stdio.h>
#include "io.h"
#include "system.h"

// klein64 parameter
#define KLEIN_ADDR_CTRL     0x00
#define KLEIN_ADDR_CONF     0x01
#define KLEIN_ADDR_STATUS   0x02

#define KLEIN_ADDR_KEY0     0x10
#define KLEIN_ADDR_KEY1     0x11

#define KLEIN_ADDR_BLOCK0   0x20
#define KLEIN_ADDR_BLOCK1   0x21

#define KLEIN_ADDR_RESULT0  0x30
#define KLEIN_ADDR_RESULT1  0x31

// blake2s parameter
#define BLAKE2S_ADDR_CTRL     0x08
#define BLAKE2S_ADDR_STATUS   0x09
#define BLAKE2S_ADDR_BLOCKLEN 0x0A

#define BLAKE2S_ADDR_BLOCK0   0x10
#define BLAKE2S_ADDR_BLOCK15  0x1F

#define BLAKE2S_ADDR_DIGEST0  0x40
#define BLAKE2S_ADDR_DIGEST7  0x47


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

// blake2s test function
void blake2s_test_RFC_7693(void) {
	// clear block
	for(int i = 0; i < 16; i++)
		IOWR(BLAKE2S_0_BASE, BLAKE2S_ADDR_BLOCK0 + i, 0x0);

	// init
	IOWR(BLAKE2S_0_BASE, BLAKE2S_ADDR_CTRL, 0x1);
	while(IORD(BLAKE2S_0_BASE, BLAKE2S_ADDR_STATUS) == 0);

	// finish
	IOWR(BLAKE2S_0_BASE, BLAKE2S_ADDR_BLOCK0, 0x61626300); // abc
	for(int i = 1; i < 16; i++)
		IOWR(BLAKE2S_0_BASE, BLAKE2S_ADDR_BLOCK0 + i, 0x0);
	IOWR(BLAKE2S_0_BASE, BLAKE2S_ADDR_BLOCKLEN, 0x3);
	IOWR(BLAKE2S_0_BASE, BLAKE2S_ADDR_CTRL, 0x4);
	while(IORD(BLAKE2S_0_BASE, BLAKE2S_ADDR_STATUS) == 0);

	// read output
	printf("\r\n----------3. Test blake2s with message in RFC 7693\r\n");
	printf("Input:    616263\r\n");
	printf("Output:   ");
	for(int i = 0; i < 8; i++) {
	  printf("%.8x", IORD(BLAKE2S_0_BASE, BLAKE2S_ADDR_DIGEST0 + i));
	}
	printf("\r\nExpected: 508c5e8c327c14e2e1a72ba34eeb452f37458b209ed63a294d999b4c86675982\r\n");
}

void main() {
	printf("Hello world. \r\n");
	klein_test();
	blake2s_test_RFC_7693();
}




