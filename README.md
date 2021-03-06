# SFND-Radar

This project is a solution for the Radar Module project in the Sensor Fusion Nanodegree

## Implementation steps for the 2D CFAR process
* Loop over elements of RDM array each iteration selecting one cell to be the CUT (Cell Under Test)<br>
`for i = Tr+Gr+1 : (Nr/2)-(Gr+Tr)`<br>
`for j = Td+Gd+1 : Nd-(Gd+Td)`
* For each iteration loop over the training cells "excluding the guarding cells" to sum their values<br>
`for p = i-(Tr+Gr) : i+(Tr+Gr)`<br>
`for q = j-(Td+Gd) : j+(Td+Gd)`
* Calculate the average of the noise value<br>
`noise_level = noise_level + db2pow(RDM(p,q));`
* Convert using pow2db<br>
`th = pow2db(noise_level/(2*(Td+Gd+1)*2*(Tr+Gr+1)-(Gr*Gd)-1));`
* Add the offset value
* If the CUT is greater then the threshold replace it by `1`, else `0` <br>
and that’s all for the Implementation.

## Selection of Training, Guard cells and offset
* `Tr = 10, Td = 8` For both Range and Doppler Training Cells.
* `Gr = 4, Gd = 4` For both Range and Doppler Guard Cells.
* `offset = 1.4` the offset value.

## Steps taken to suppress the non-thresholded cells at the edges
This was done throught sclicing the output such that we have the surrounding rows and columns depending on the Training cells for both range and doppler.<br>
`RDM(union(1:(Tr+Gr),end-(Tr+Gr-1):end),:) = 0;  % Rows`<br>
`RDM(:,union(1:(Td+Gd),end-(Td+Gd-1):end)) = 0;  % Columns`

## Output:
This is the output for a target at 150m moving at -50 m/s relative speed<br><br>

### Range from 1D FFT
![alt text](https://github.com/AkshayPR244/UdacitySFND-Radar/blob/main/Range_from_1D_FFT.png)

### Range and Speed from 2D FFT
![alt text](https://github.com/AkshayPR244/UdacitySFND-Radar/blob/main/RangeSpeed_from_2D_FFT.png)

### Output from Cell Averaging CFAR on 2D FFT
![alt text](https://github.com/AkshayPR244/UdacitySFND-Radar/blob/main/CA_CFAR_2D.png)
