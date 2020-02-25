%% Experiment 1 Simpele destillatie radfrac water-methanol
%%Link
Aspen = actxserver('Apwn.Document.36.0');
Simulation_Name = 'Simulation 1';
Aspen.invoke('InitFromArchive2', ['C:\Users\Jordy\Dropbox\Masterproef\Experimenten\Data 2\' Simulation_Name '.bkp']);
Aspen.Visible=1;
Aspen.SuppressDialogs = 1;
Aspen.Engine.Run2(1);
while Aspen.Engine.IsRunning == 1
   pause(0.5);
end
nonConvergence=[];

%% Datacollectie
%% Input = 3 param: RR, D:F en flow rate

Input = [];
Results=[];
aantal=0;

prompt = "Hoeveel experimenten uitvoeren? ";

aantal= uint16(input(prompt));
 if isinteger(aantal)
        else 
            disp("Dit is geen natuurlijk getal.");
            prompt = "Hoeveel experimenten uitvoeren? ";

            aantal= uint16(input(prompt));
 end
aantal
for i=1:aantal

   
    
    
    RandomReflux= 0.01+ (5-0.01)*rand(1,1);
    RandomDF= 0.01+ (0.99-0.01)*rand(1,1);
    RandomFeedStage= ceil(1+(21-1)*rand(1,1));
   
    
    Input(i,:) = [RandomReflux; RandomDF; RandomFeedStage];

end


Input

%% Aspen

for n=1:length(Input)
    try
Aspen.Tree.FindNode("\Data\Blocks\B1\Input\BASIS_RR").Value=Input(n,1);
    
    Aspen.Tree.FindNode("\Data\Blocks\B1\Input\D:F").Value=Input(n,2);
        
    Aspen.Tree.FindNode("\Data\Blocks\B1\Input\FEED_STAGE\IN").Value=Input(n,3);
       
        Aspen.Reinit;
        Aspen.Engine.Run2(1);
         time = 1;
        while Aspen.Engine.IsRunning == 1 % 1 --> If Aspen is running; 0 ---> If Aspen stop.
        pause(0.5);
        time = time+1;
            if time==15 % Control of simulation time.
            Aspen.Engine.Stop;
            end 
        end
         Simulation_Convergency = Aspen.Tree.FindNode("\Data\Results Summary\Run-Status\Output\PCESSTAT").Value; % 1 Doesn't Convergence; 0 Converge
         Results1(n,1) = Aspen.Tree.FindNode("\Data\Streams\DEST\Output\MOLEFRAC\MIXED\BENZE-01").Value;
         Results1(n,2) = Aspen.Tree.FindNode("\Data\Streams\DEST\Output\MOLEFRAC\MIXED\TOLUE-01").Value;
         Results1(n,3) = Aspen.Tree.FindNode("\Data\Streams\DEST\Output\MOLEFRAC\MIXED\P-XYL-01").Value;
         Results1(n,4) = Aspen.Tree.FindNode("\Data\Streams\BOTTOM\Output\MOLEFRAC\MIXED\BENZE-01").Value;
         Results1(n,5) = Aspen.Tree.FindNode("\Data\Streams\BOTTOM\Output\MOLEFRAC\MIXED\TOLUE-01").Value;
         Results1(n,6) = Aspen.Tree.FindNode("\Data\Streams\BOTTOM\Output\MOLEFRAC\MIXED\P-XYL-01").Value;
         Results1(n,7) = Aspen.Tree.FindNode("\Data\Blocks\B1\Output\COND_DUTY").Value;
         Results1(n,8) = Aspen.Tree.FindNode("\Data\Blocks\B1\Output\REB_DUTY").Value;
         
        disp("Completed simulation");
        n
    catch ME
        warning('Cannot converge');
        nonConvergence = [nonConvergence;n]
    end
        
end

Results1
InputTitle={'RefluxRatio', 'DestillateToFlow', 'FeedStage'};
ResultTitle={'XDESTBENZE', 'XDESTTOL', 'XDESTXYL', 'XBOTBENZE', 'XBOTTOL', 'XBOTXYL','CONDDUTY', 'REBDUTY'};
Results=[Input Results1];
ResultsTitle=[InputTitle ResultTitle];
csvwrite_with_headers('InputDataCasestudy1.csv',Results,ResultsTitle)
Aspen.Close;
Aspen.Quit;
     
    
