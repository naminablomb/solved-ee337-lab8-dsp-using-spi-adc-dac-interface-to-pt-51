Download Link: https://assignmentchef.com/product/solved-ee337-lab8-dsp-using-spi-adc-dac-interface-to-pt-51
<br>



<ol>

 <li>Write driver code for interfacing DAC (TLV5616) with Pt-51.</li>

 <li>Testing DAC (TLV5616) for a simple application for waveform generation.</li>

 <li>Integrating DAC (TLV5616) and ADC (TLV1543) for implementing a moving averagefilter.</li>

</ol>

<h1>Background</h1>

We have used SPI (Serial Peripheral Interface) for interfacing ADC TLV1543 to Pt-51. In this week’s experiment we shall use SPI to connect TLV5616 to Pt-51. The TLV5616 is a 12-bit DAC with 4-wire serial peripheral interface(SPI). The TLV5616 is programmed with a 16-bit serial string containing 4 control and 12 data bits.

In this experiment, we shall interface the DAC with Pt-51 to generate basic waveforms (ramp and sinusoidal) and observe them on DSO. We shall then use DAC, ADC and Pt-51 for implementing moving average filter.

<h1>Details regarding the DAC interfacing</h1>

The TLV5616 uses 4 wire lines for serial interfacing. The SCLK is used for clocking the chip and synchronizing it. DIN is the pin through which the 4 bit control signals along with the 12 bit data signal is received serially. The Frame Sync signal, FS, should be issued as per the timing diagram requirement to get the chip running. The chip select signal is used to select the chip when we connect and communicate with multiple chips using SPI protocol.

Note that here the data sampling happens at the falling edge of the SCLK signal. Therefore while making the configurations, also make the necessary changes to meet this requirement.

In the power down mode, all the amplifiers within the TLV5616 are disabled.

We would like to thank Texas Instruments for providing the DAC (TLV5616) for this experiment!

Figure 1: Timing diagram of TLV5616.

Figure 2: Data format.

The output voltage (full scale determined by external reference) is given by:

(1)

where <em>V</em><sub>ref </sub>is the reference voltage and <em>V<sub>digital </sub></em><em><sub>in </sub></em>is the digital input value within the range of 0 to 2<sup>(<em>n</em>−1)</sup>, where n = 12 (bits).

<h1>Homework</h1>

<ol>

 <li>Read the datasheet of TLV 5616 for details from http://www.ti.com/product/TLV5616/ technicaldocuments</li>

 <li>Calculate the theoretical analog output values that can be obtained from the DAC forthe following digital inputs(and <em>V<sub>CC </sub></em>= 3<em>.</em>3<em>V </em>):</li>

</ol>

<table width="327">

 <tbody>

  <tr>

   <td width="117"><strong>Digital input</strong></td>

   <td width="209"><strong>Expected output voltage</strong></td>

  </tr>

  <tr>

   <td width="117">0x000</td>

   <td width="209"> </td>

  </tr>

  <tr>

   <td width="117">0x400</td>

   <td width="209"> </td>

  </tr>

  <tr>

   <td width="117">0x800</td>

   <td width="209"> </td>

  </tr>

  <tr>

   <td width="117">0xFFF</td>

   <td width="209"> </td>

  </tr>

 </tbody>

</table>

<ol start="3">

 <li>Calculate the theoretical digital input values for which DAC give the following analogoutputs( and <em>V<sub>CC </sub></em>= 3<em>.</em>3<em>V </em>):</li>

</ol>

<table width="321">

 <tbody>

  <tr>

   <td width="130"><strong>Analog output</strong></td>

   <td width="191"><strong>Required digital input</strong></td>

  </tr>

  <tr>

   <td width="130">1V</td>

   <td width="191"> </td>

  </tr>

  <tr>

   <td width="130">1.66V</td>

   <td width="191"> </td>

  </tr>

  <tr>

   <td width="130">3.3V</td>

   <td width="191"> </td>

  </tr>

 </tbody>

</table>







<h1>Lab Work</h1>

<ol>

 <li><strong>Interfacing TLV5616 to Pt-51</strong>

  <ul>

   <li>Complete the function <em>spi init() </em>to configure SPCON, IENO and IEN1 registers of AT89C5131.

    <ol>

     <li>Microcontroller SPI module as Master</li>

     <li>Free the SS pin (microcontroller does not require chip select as we are con-figuring it in master mode)</li>

    </ol></li>

  </ul></li>

</ol>

<ul>

 <li>Select the Master clock rate as Fclk/16 (by choosing appropriate values of</li>

</ul>

SPR0, SPR1, SPR2 we can get different baud rates) iv. Select appropriate setting for serial clock polarity and clock phase. Refer timing diagrams (Figures 3 and 4 in this handout) of SPI communication of microcontroller and TLV5616 (Refer datasheets of both ICs for more information).

<ol>

 <li>Enable SPI module of the microcontroller.</li>

 <li>Enable SPI interrupt (<strong>IEN1</strong>-Interrupt enable register) (Read the datasheet of AT89C5131a for more information)</li>

</ol>

<ul>

 <li>Set enable all interrupt (EA) bit (<strong>IEN0</strong>– Interrupt enable register) (Refer datasheet of AT89C5131a for more information)</li>

</ul>

<strong>Hint: </strong>Compare the timing diagrams of ADC (TLV1543) and DAC (TLV5616), you will see that there is a difference between the two. Note that the data is read by the peripheral in falling SCK edge for one and in the rising SCK edge for the other. SPCON values have to be set with this in mind.

<ul>

 <li>Complete the function <em>dac() </em>in tlv5616.h. Refer the timing diagram to understand:

  <ol>

   <li>Make <em>cs bar dac </em>and <em>fs </em>low</li>

   <li>Write data into <em>temp dac data</em></li>

  </ol></li>

</ul>

<ul>

 <li>Make <em>cs bar dac </em>and <em>fs </em>high</li>

</ul>

<ul>

 <li>Testing your code: Make the connections as shown in Fig. 3.</li>

 <li>Test the analog output for the digital values corresponding to the test cases mentioned in the homework part 2.</li>

 <li>Write a program to generate a symmetrical triangular signal that varies from 0 to 3.3V with a period of 12sec (6sec+6sec)</li>

</ul>

Figure 3: DAC connection diagram.

<ol start="2">

 <li><strong>Interfacing ADC and DAC together</strong></li>

</ol>

Make the connections as shown in Figure 4. This is an application where a master (Pt-51) is connected to two SPI slaves (ADC and DAC). For this we will require two Slave Select instead of one.

Figure 4: ADC-DAC circuit diagram

You need to make the following changes for making the entire system work:

<ul>

 <li>Make changes to functions <strong>adc() </strong>(in tlv1543.h) and <strong>dac() </strong>(in tlv5616.h) such that the SPCON register is changed to their respective values for each peripheral (<em>spi init() </em>need not be changed)</li>

 <li>Make changes to <strong>main() </strong>function such that data is read from adc() and written to dac()</li>

</ul>

(Note that tlv1543 is 10bit ADC and tlv5616 is a 12bit DAC. How will you deal with this ?)

<ul>

 <li>Use sine wave (5Hz to 600Hz, 0 to 3V) as input to ADC and observe the outputfrom DAC on the DSO.</li>

</ul>

<ol start="3">

 <li><strong>Implementing Moving Average filter: A DSP application</strong></li>

</ol>

Moving average filter is simple finite length filter that can be used to remove noise. It can be implemented by computing the average of N successive samples of the input and is given by the following equation:

Example, for N=4

It can be implemented in a computationally efficient way by successively updating the moving average from the previous instance using the following equation:

<ul>

 <li>Make changes to main()

  <ol>

   <li>Take data from ADC</li>

   <li>Use the function <strong>moving avg() </strong>(in filters.h) to obtain the moving average</li>

  </ol></li>

</ul>

<ul>

 <li>Give data to DAC</li>

</ul>

<ul>

 <li>Plot the frequency response (magnitude only) for the above filter by sweeping theinput frequency from 5Hz to 600Hz.</li>

</ul>

<strong>Function descriptions</strong>

This section describes all the functions required to be written for this interfacing experiment. Note that each header file and functions in them perform specific tasks. The functions are written such that they take input data from other function and return processed data.

<ol>

 <li><strong>main.c :</strong></li>

</ol>

This file includes all necessary header files which have function definitions that will be used. The microcontroller executes <em>main() </em>function from this file. This function calls all necessary functions sequentially. <em>main.c </em>have to be changed according to the requiremnet in the problem statement. Outline of the changes to be made for each part of the problem statement is as follows:

Part 1: It calls initialization functions for all peripherals which are <em>spi init() </em>(for SPI module of Microcontroller AT89C5131), <em>dac init()</em>(for DAC TLV5616)

1(d) Call <em>dac() </em>function for communicating with the DAC. Pass as parameters the digital values that are given in homework and verify the output voltage.

Figure 5: Software Architecture

1(e) These steps are repeated infinitely using while loop so that triangle wave in question 1(e) will be generated continuously:

Call <em>dac() </em>function for communicating with the DAC and pass as argument digital values:

<ol>

 <li>If the ramp is in the increasing cycle, the digital value have to beincremented after appropriate dalay and passed as argument.</li>

 <li>If the ramp is in the decreasing cycle, the digital value have to bedecremented after appropriate dalay and passed as argument.</li>

</ol>

Part 2: It calls initialization functions for all peripherals which are <em>spi init() </em>(for SPI module of Microcontroller AT89C5131), <em>dac init() </em>(for DAC TLV5616) and <em>adc init() </em>(for ADC IC TLV1543)

These steps are repeated infinitely using while loop so that the input given to ADC will be sampled and passed to DAC continuously

<ol>

 <li>The data returned by adc() is read</li>

 <li>The digital data is passed to dac()</li>

</ol>

Part 3: It calls initialization functions for all peripherals which are <em>spi init() </em>(for SPI module of Microcontroller AT89C5131), <em>dac init() </em>(for DAC TLV5616) and <em>adc init()</em>

(for ADC IC TLV1543)

These steps are repeated infinitely using while loop so that the input given to ADC will be sampled and passed to DAC continuously

<ol>

 <li>The data returned by adc() is read</li>

 <li>The read data is passed as argument to moving avg() and the return value from it is stored</li>

</ol>

<ul>

 <li>The digital data returned after moving average calculation is passed todac()</li>

</ul>

<ol start="2">

 <li><strong>h :</strong></li>

</ol>

Functions in this file control all operations related to AT89C5131 SPI module. Task of this module is to facilitate SPI communication between master (Microcontroller) and slave (SPI device – ADC). Data from all interfaced SPI devices are gathered by calling function from this file and passed to other functions for further processing. So if we have to switch to another microcontroller platform, only <strong>spi.h </strong>(driver) file needs to be changed and processing functions can remain the same. The file <strong>spi.h </strong>contains following functions.

<ul>

 <li><em>spi init():</em>

  <ol>

   <li>Parameters to be passed: None</li>

   <li>Parameters to be returned: None</li>

  </ol></li>

</ul>

<ul>

 <li>Task:</li>

</ul>

Function initialises SPI module in AT89C5131 by configuring SPCON, IEN0 and IEN1 register.

<ul>

 <li><em>spi trx 16 bit()</em>:

  <ol>

   <li>Parameters to be passed: unsigned int – 16 bit data to be sent from masterto slave. ii. Parameters to be returned: unsigned int – 16 bit data from slave.</li>

  </ol></li>

</ul>

iii. Task:

In AT89C5131, spdat (Transmit and receive data buffer) is an 8 bit SFR which allows 8 bit data to be transmitted or received. To transmit or receive 16 bit data, two 8 bit data frames may be used one after the other. 16 bit data passed to this function from other functions will be unpacked (split) into two bytes. These two bytes are transmitted to slave, simultaneously master will receive two bytes from the slave. Note that most significant byte is transmitted first and then the least significant byte. Similarly, the master will receive the most significant byte first and then the least significant byte. These two received bytes are then packed (joined) into 16 bit data. This data word will be returned by this function.

<ul>

 <li><em>spi interrupt()</em>:

  <ol>

   <li>Parameters to be passed: Noneii. Parameters to be returned: None iii. Task: This is the ISR (Interrupt Service Routine) for SPI interrupt. This function will be called after completion of transfer of 8 bits of data to the slave. The 8 bits received from the slave are stored in a temporary global variable which will be used by <em>spi trx 16 bit() </em></li>

  </ol></li>

</ul>

<ol start="3">

 <li><strong>h:</strong></li>

</ol>

Functions in this file are specific to TLV1543. These functions control and enable data transfer between microcontroller and ADC slave. Handling of control signals and data processing specific to this IC is facilitated by this header file. Functions from <strong>spi.h </strong>are called by functions in this file. Hence <strong>spi.h </strong>is included in <em>tlv1543.h</em>. Configuration parameters specific to TLV1543 are set in function in this file. Following are functions in this file.

<ul>

 <li><em>adc init()</em>:

  <ol>

   <li>Parameters to be passed: None</li>

   <li>Parameters to be returned: None</li>

  </ol></li>

</ul>

<ul>

 <li>Task:</li>

</ul>

Control signals related to TLV1543 are initialized here.

<ul>

 <li><em>adc()</em>:

  <ol>

   <li>Parameters to be passed: None</li>

   <li>Parameters to be returned: unsigned int – two bytes of data correspondingto the analog input received. iii. Task:</li>

  </ol></li>

</ul>

This function triggers communication between microcontroller and ADC slave. Following tasks are executed sequentially.

<ul>

 <li>Multiple slaves (if connected) might need different settings of configuration of same SPI module of master. These slave specific requirements are taken care of by functions in slave specific driver (Here <em>adc()</em>).</li>

 <li>Enable ADC slave.</li>

 <li>A two byte data frame having address of input channel A0 is formed (Check timing diagram of SPI communication for this). This data is then passed to function <em>spi trx 16 bit()</em>.</li>

 <li><em>spi trx 16 bit() </em>will take care of SPI communication between microcontroller and ADC slave and return sampled ADC value from that channel. 5) Disable ADC slave.</li>

</ul>

6) Data word returned by function <em>spi trx 16 bit() </em>is processed to make it in correct format (Check timing diagram of SPI communication for this).

This data word is then returned by this function.

<ol start="4">

 <li><strong>h:</strong></li>

</ol>

Functions in this file are specific to TLV5616. These functions control and enable data transfer between microcontroller and DAC slave. Handling of control signals and data processing specific to this IC is facilitated by this header file. Functions from <strong>spi.h </strong>are called by functions in this file. Hence <strong>spi.h </strong>is included in <em>tlv5616.h</em>. Configuration parameters specific to TLV5616 are set in function in this file. Following are functions in this file.

<ul>

 <li><em>dac init()</em>:

  <ol>

   <li>Parameters to be passed: None</li>

   <li>Parameters to be returned: None</li>

  </ol></li>

</ul>

<ul>

 <li>Task:</li>

</ul>

Control signals related to TLV5616 are initialized here.

<ul>

 <li><em>dac()</em>:

  <ol>

   <li>Parameters to be passed: unsigned int – two bytes of data corresponding to</li>

  </ol></li>

</ul>

the DAC analog output.

<ol>

 <li>Parameters to be returned: None</li>

</ol>

<ul>

 <li>Task:</li>

</ul>

This function triggers communication between microcontroller and DAC slave. Following tasks are executed sequentially.

<ul>

 <li>Multiple slaves (if connected) might need different settings of configuration of same SPI module of master. These slave specific requirements are taken care of by functions in slave specific driver (Here <em>dac()</em>).</li>

 <li>Send appropriate control signals for starting communication to DAC slave (DAC Chip select,frame sync etc).</li>

 <li>A two byte data frame having 4 control bits and 12 DAC output value bits is formed from 16 bit data word passed to this function from other function (Check TLV5616 datasheet for more information).Select appropriate control bits. This data is then passed to function <em>spi trx 16 bit()</em>.</li>

 <li><em>spi trx 16 bit() </em>will take care of SPI communication between microcontroller and DAC slave and return garbage data word since DAC do not return any value. Flush returned data word.</li>

 <li>Send appropriate control signals for stopping communication to DAC slave (DAC Chip select,frame sync etc).</li>

</ul>

<ol start="5">

 <li><strong>h:</strong></li>

</ol>

Function in this file is used to compute the moving average of samples passed as argument. It is in this function that the number of samples that is used to compute the moving average is specified (<em>f no samples</em>). We have specified it to be eight, but this can changed according to requirement.

<em>moving avg():</em>

<ol>

 <li>Parameters to be passed: Current Sample</li>

 <li>Parameters to be returned: Moving average of N samplesiii. Task: Computing the moving average for the specified number of samples</li>

</ol>