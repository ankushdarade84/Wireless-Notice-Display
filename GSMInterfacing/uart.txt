
void uart_init(void)	   // INITIALIZE SERIAL PORT
{
	TMOD = 0x20;	  // Timer 1 IN MODE 2-AUTO RELOAD TO GENERATE BAUD RATE
	TH1 = 0xFD;	// LOAD BAUDRATE TO TIMER REGISTER
	SCON = 0x50;	  // SERIAL MODE 1, 8-DATA BIT 1-START BIT, 1-STOP BIT, REN ENABLED
	TR1 = 1;			      // START TIMER
}
void tx_data(unsigned char serialdata)
{
	SBUF = serialdata;			// LOAD DATA TO SERIAL BUFFER REGISTER
	while(TI == 0);				// WAIT UNTIL TRANSMISSION TO COMPLETE
	TI = 0;		// CLEAR TRANSMISSION INTERRUPT FLAG
}
unsigned rx_data(void)	
{
	while(RI == 0);				// WAIT UNTIL DATA IS RECEIVED 
	RI = 0;					// CLEAR FLAG
	return SBUF;				// RETURN SERIAL DATA
		
}
void tx_string(unsigned char *str)
{
	while(*str)
	{
		tx_data(*str);
		str++;
	}
	
}
// END OF PROGRAM