                                             VTX GURU     
                                      FPV IMD Analysis Tools




	VTX Guru scores FPV VTX channel groups based on the IMD (Inter Modulation Distortion)
	profile of the channel group. Additionally, VTX Guru may progressively reduce
	the output score based upon the VTX channel separation within the group. Video Clarity 
	Score (VCS) term is used to indicate that score may not have been strictly IMD derived; 
	it may have been influenced by the VTX channels separations. This is an attempt to 
	allow comparison of score alone to be more meaningful.
	


	IMD SCORE
	---------
	The IMD Score is calculated based on a unique algorithm, which borrows from a prior IMD
	calculator the idea that when an resultant IMD frequency is within 35MHz of a FPV VTX
	channel we can consider it a problem, it is scored  by subtracting the separation to the 
	nearest VTX channel from 35, and the score is reduced from the top possible score - 100. 
	The previous calculator and corresponding explanation are at http://www.etheli.com/IMD/.

	With VTX Guru, unlike it's predecessor, scores strictly go from 100 (highest) to
	0 (lowest). Not every score of 0 is equally bad, but rather it indicates a fail condition
	has been meet. When scores near perfect (100), a greater discernment between similar 
	performing groups can be observed.

	To begin calculating a score, we look at each resultant IMD frequency for a 5GHz VTX freq 
	group. This is done with the standard IMD formula -  F3 = (F1 x 2) - F2.  If an  
	IMD freq is an exact match to a VTX channel, a fail condition is met and the score goes 
	to zero. Otherwise, if a IMD freq is within 35MHz of a VTX channel, it is considered a 
	"problem IMD frequency". A problem IMD freq. can be within 35MHz of more than one VTX 
	channel. For each problem IMD frequency - the least separation (sep.) to a VTX channel 
	(in MHz) is noted, and how many times the separation was less than 35 MHz to a channel
	(IMD_close_to_chan).

	The least separation to a VTX channel is subtracted from 35 resulting in a "reducing score"
	(for each problem IMD freq) that ranges from 1 (34MHz sep.) to 34 (1MHz sep.). Furthermore,
	a "reducing factor" is created based on how many times in total problem IMD frequencies 
	were close to VTX channels, for the entire group: 


	reducing factor =  1 - (IMD_close_to_chan / 30)


	Then calculated is the root mean square (RMS) of the reducing scores. The worst possible 
	result would be a RMS of 35, what all exact IMD matches to VTX channels would produce. 
	The RMS of reducing scores is then scaled by a factor such that this worst result (35) 
	becomes 100  --   35 X (scale factor) =  100;  thus a zero score is produced in this worst 
	case as IMD Score is calculated as such:
	
	 
	IMD Score =  (100 - scaled RMS of reducing scores) X reducing factor


	Also calculated is an alternate IMD score derived from the RMS of reducing scores of unique 
	problem IMD frequencies. If this alternate IMD score is lower than the first it is used in 
	it's place. In instances when the same IMD frequency is found more than once in the IMD 
	frequency matrix this prevents the score from being improved by the duplicate IMD frequency. 	



	BROADCAST FACTOR
	----------------
	


	The IMD score is next multiplied by a "broadcast factor", if one has been created, to 
	penalize the score proportional to the VTX channels separations. 

	Video Clarity Score = IMD Score X Broadcast Factor

	A broadcast factor will be created if any VTX channels are closer than 40 MHz apart.
	If any VTX channels are less than 28 MHz apart a fail condition is met and the score goes 
	to zero. Otherwise, if VTX channel separations are lower than 40 MHz the closest 
	separation is noted and all channel separations under 40 MHz counted (broadcast_close_times). 
	By subtracting closest separation from 40, a reducing score is created from 1 to 12 
	(broad_score). Broadcast factor is calculated as such:


	broadcast_factor = (1 - (broad_score / (26 - broad_score)))


	But if closest VTX separation was below 37MHz - instead:

	broadcast_factor = (1 - (broad_score / (26 - broad_score))) * (1/broadcast_close_times)


	As you can see if any VTX channels separation is less than 37 MHz - an additional 
	factor is multiplied to the broadcast factor. In this case, if broadcast_close_times is 
	greater than one - broadcast factor is greatly reduced. Allowing only one 
	broadcast_close_time without more than halving the Video Clarity Score. So anytime 
	a group is listed in the Charts with a VTX separation of less than 37, it is the only
	VTX separation under 40 MHz for that group.

	

	WEIGHTED SCORE
	--------------
	Additionally, 5 and 6 channel groups (comprised from 40 channel set) cannot score perfect 
	100 scores. So to make things easier the weighted scoring for 5 and 6 channel groups was 
	created. With weighted scoring 	the "best in class" 40 channel group gets a weighted score 
	of 100. The seeming least good of previously recommended channel groups (IMD5 & IMD6) 
	both get a score of 62.3 weighted. The Video Clarity Score is scaled appropriately to 
	achieve this result.

	With weighted scoring we can call 62.3  (or slightly below at 60, as I have done)
	the fail point. So all scores above 60 are "Pass". It is these groups with Pass scores 
	that compromise	the database searched in Smart Search and shown in Charts. 

	With 3 and 4 channel groups, and with 5 channel groups with lowband, the same above 
	60 score = Pass method is used, however the unweighted Video Clarity Score is used. In this
	case, to judge from sixty and above is more arbitrary; it's done for consistency of the above 
	60 pass logic and ensures sufficient entries for Smart Search to search. Always use groups 
	nearing the fail point (60) with caution.

	With 6 channel groups with lowband - weighted scores of 83.3 and above populate the pass
	list. This is done to match the 5 channel with lowband list, who's lowest passing group
	has a weighted score of 83.3. This way when recommending lowband groups, the groups recommended
	will be either better than or very good when compared to non-lowband 5 & 6 groups.



	Adjustments
	-----------
	The preceding equations and methods have lead to results that seem to make sense generally. 
	Of course, adjustments can be made to better track the video clarity relationship between 
	the broadcast (VTX) channels separations and the resultant IMD profile; and to improve
	the IMD scores calculation. I am not a R.F. professional; this is an experimental approach.

	One way to observe the current adjustments being made by broadcast factor is to open the 
	Charts tool and select for 5 pilots then select for the 40 VTX channel study set -  
	(i.e. NOT USA only and NOT lowband). You should be able to see that the 7th best group in the 
	list has a better IMD Score than any other group, but it's Video Clarity Score (labeled SCORE) 
	has been lowered past the above groups because of the broadcast factor. Whether this group 
	has been moved down the correct amount is certainly a point of consideration concerning tuning 
	the broadcast factor; as well for determining the best 5 channel group from the 40 channel 
	study set.

	Also, the reducing factor that is used to calculate the IMD Score is a major candidate to 
	be adjusted; to alter how much the IMD score diminishes with additional IMD_close_to_chan 
	events.

	The algorithm is currently tuned to give relevant results with 2 to 6 pilots flying at 
	once. To be able to better judge larger numbers of pilots flying at once the broadcast factor
	and the reducing factor would need to be adjusted.


