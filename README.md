# Radar-Target-Generation-Detection

## Code Starter

See https://video.udacity-data.com/topher/2019/May/5ce36479_radar-target-generation-and-detection/radar-target-generation-and-detection.m

## 2D CFAR Process

### Step 1

Determine the number of Training & Guard cells for each dimension.

% ** :
%Select the number of Training Cells in both the dimensions.
Tr = 10;
Td = 8;

% ** :
%Select the number of Guard Cells in both dimensions around the Cell under 
%test (CUT) for accurate estimation
Gr = 4;
Gd = 4;

% ** :
% offset the threshold by SNR value in dB
offset = 6;

### Step 2-7 (the main loop)

(2) Slide the cell under test across the complete matrix. Make sure the CUT has margin for Training and Guard cells from the edges.

(3) For every iteration sum the signal level within all the training cells. To sum convert the value from logarithmic to linear using db2pow function.

(4) Average the summed values for all of the training cells used. After averaging convert it back to logarithmic using pow2db.

(5) Further add the offset to it to determine the threshold.

(6) Next, compare the signal under CUT against this threshold.

(7) If the CUT level > threshold assign it a value of 1, else equate it to 0.



% Step (2)

for i = Tr+Gr+1 : Nr-(Tr+Gr)

    for j = Td+Gd+1 : Nd-(Td+Gd)
    
        noise_level = zeros(1,1);
   
        for p = i-(Tr+Gr) : i+Tr+Gr
        
            for q = j-(Td+Gd) : j+Td+Gd 
            
                if (abs(i-p)>Gr || abs(j-q)>Gd)
                
                    noise_level = noise_level + db2pow(RDM(p,q)); % Step (3)
                    
                end
                
            end
            
        end
    
        % Steps (4) and (5)
        
        threshold = pow2db(noise_level/(2*(Td+Gd+1)*2*(Tr+Gr+1) - (Gr*Gd) - 1)) + offset;
        
        % Steps (6) and (7)
        
        if(RDM(i,j)>threshold)
            RDM(i,j) = 1;
        else
            RDM(i,j) = 0;
        end

    end
    
end


### Step 8

To keep the map size same as it was before CFAR, equate all the non-thresholded cells to 0.

RDM(RDM ~= 0 & RDM ~= 1) = 0;
