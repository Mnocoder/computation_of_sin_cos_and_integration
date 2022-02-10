l1 = [ "0   ", "pi/6", "pi/4", "pi/3", "pi/2"];
l2 = [ "2*pi/3  ", "pi      ", "2*pi    ", "0.429*pi", "0.683*pi"];
terms = 20;

[sinout,cosout,tanout] = plotting(l1,terms);
[sinout,cosout,tanout] = plotting(l2,terms);

function [sinval,cosval,tanval] = plotting(l,terms)
    x = [];
    for i  = 1 : length(l)
        x = [x str2num(l(i))]; %Converting each string in list into number
    end
    sinval = []; cosval = []; tanval = [];
    for  j  = 1:3
        figure % new figure for each iteration
        if j == 1; disp("  sin - convergent term - value");
        elseif j == 2; disp("  cos - convergent term - value");
        else;  disp("  tan - convergent term - value"); 
        end
        for i = 1:length(x)
            if j == 1
                sin = sine(terms,x(i)); %list of sin function values
                term = converge(sin); %convergent term index
                sinval = [sinval sin(term)]; %storing convergent sin value
                fprintf('\t%s : %d\t : %f\n', l(i),term,sinval(i));
                plot(1:terms,sin); title('sin x');
            elseif j == 2
                cos = cosine(terms,x(i)); %list of cos function values
                term = converge(cos); %convergent term index
                cosval = [cosval cos(term)]; %storing convergent cos value
                fprintf('\t%s : %d\t : %f\n', l(i),term,cosval(i));
                plot(1:terms,cos); title('cos x');
            else
                tan = tangent(sine(terms,x(i)), cosine(terms,x(i)));
                term = converge(tan); %convergent term index
                tanval = [tanval tan(term)]; %storing convergent tan value
                fprintf('\t%s : %d\t : %f\n', l(i),term,tanval(i));
                plot(1:terms,tan); title('tan x');
            end
            hold on % to plot for various values
        end
        legend(l); xlabel('Number of terms'); ylabel('Function Value');
        hold off
    end
end


function sinans = sine(terms,x)
    sinans = [];  sinans = [sinans x]; % first term = x
    for j = 3:2:((terms-1)*2+1) % j = 3 to 39
        % sum of all previous values + (-1^(j-1)/2) * (x^j)/j!
        % (j-1)/2 makes 3,5,7 as 1,2,3 ,so power will be odd,even,odd 
        % which creates -,+,-,+ for consecutive terms
        newval = sinans(end) + ((-1)^((j-1)/2)) * ((x^j)/factorial(j));
        sinans = [sinans newval];
    end
end

function cosans = cosine(terms,x)
    cosans = [];  cosans = [cosans 1]; % first term = 1
    for j = 2:2:((terms-1)*2) % j = 2 to 38
        % sum of all previous values + (-1^j/2) * (x^j)/j!
        % j/2 makes 2,4,6 as 1,2,3 ,so power will be odd,even,odd 
        % which creates -,+,-,+ for consecutive terms
        newval = cosans(end) + ((-1)^(j/2)) * ((x^j)/factorial(j));
        cosans = [cosans newval];
    end
end   

function tanans = tangent(sin,cos)
    tanans = sin./cos; % each term in sin / each term in cos
end

function convans = converge(series)
    for i = 1 : length(series)-2
        if  series(i+2)-series(i+1) == series(i+1)-series(i)
            % if next - current  = current - prev
            convans = i;  break;
        else
            convans = i+2; % else last term(no. of terms)
        end
    end
end


