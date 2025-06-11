---
title: Status codes
description: Information about status codes in Cognite Data Fusion.
---

Status codes are used to determine the quality of time series data points.
The status code impacts data retrieval, aggregate compuation, and how the time series should be visualized.
The implementation follows the OPC UA standard, and both the [Cognite OPC UA][opcua-extractor] and [PI extractors][pi-extractor] support this feature.

By default, the time series API only includes data points where the status code is _Good_ and disregards other data points.
To include data points with other status codes, you must pass additional parameters to the API.

[opcua-extractor](https://docs.cognite.com/cdf/integration/guides/extraction/opc_ua/)
[pi-extractor](https://docs.cognite.com/cdf/integration/guides/extraction/pi)

## Main status codes

All status codes fall into one of the main categories: _Good_, _Uncertain_, or _Bad_.
If status code is missing, it is assumed to be _Good_ (with code = 0).

The API uses decimal numbers to represent status codes, but we have included the hexadecimal representation for reference.

<table>
<tr><th>Symbolic representation</th><th>Code (decimal)</th><th>Code (hexadecimal)</th></tr>
<tr><td>Good</td><td>0</td><td>0x00000000</td></tr>
<tr><td>Uncertain</td><td>1073741824</td><td>0x40000000</td></tr>
<tr><td>Bad</td><td>2147483648</td><td>0x80000000</td></tr>
</table>

### Good

_Good_ status codes are the default, and indicate that the data point is valid. If status
code is missing, it is assumed to be _Good_ with code = 0.

### Bad

_Bad_ status codes are by default omitted from the responses, and indicate that the value cannot be trusted.

### Uncertain
_Uncertain_ status codes may be treated as _Bad_ (default) or _Good_, depending on the user.

## Status code structure
A status code consists of a 32-bit unsigned integer _code_, and a string _symbolic representation_.

A permitted status code consists of a _category_ and a set of _modifier flags_ or _info flags_.
Use the category as is, or combine it with the modifier flags to get the full status code.

To obtain the full numeric code,
you add (or bitwise OR) the values for the category and the modifier flags.
To obtain the full symbolic representation, you concatenate the representations for the category and the modifier flags.

The code fields can be added together, while the representations can be concatenated,
starting with the category and then the modifiers in order.

For instance is `"BadBrowseNameInvalid, StructureChanged, Low"` = 2153775104 + 32768 + (1024) + 256 = 2153809152.
The 1024 is added when _info flags_ are used. Here we have set Limit to Low.

## Subcategories
<details>
<summary>Click to see full list of subcategories</summary>
<table>
<tr><th>Category symbolic representation</th><th>Code (decimal)</th><th>Code (hexadecimal)</th></tr>
<tr><th>Good</th><th></th><th>0x0...0000</th></tr>
<tr><td>Good</td><td>0</td><td>0x00000000</td></tr>
<tr><td>GoodCallAgain</td><td>11075584</td><td>0x00A90000</td></tr>
<tr><td>GoodCascade</td><td>67698688</td><td>0x04090000</td></tr>
<tr><td>GoodCascadeInitializationAcknowledged</td><td>67174400</td><td>0x04010000</td></tr>
<tr><td>GoodCascadeInitializationRequest</td><td>67239936</td><td>0x04020000</td></tr>
<tr><td>GoodCascadeNotInvited</td><td>67305472</td><td>0x04030000</td></tr>
<tr><td>GoodCascadeNotSelected</td><td>67371008</td><td>0x04040000</td></tr>
<tr><td>GoodClamped</td><td>3145728</td><td>0x00300000</td></tr>
<tr><td>GoodCommunicationEvent</td><td>10944512</td><td>0x00A70000</td></tr>
<tr><td>GoodCompletesAsynchronously</td><td>3014656</td><td>0x002E0000</td></tr>
<tr><td>GoodDataIgnored</td><td>14221312</td><td>0x00D90000</td></tr>
<tr><td>GoodDependentValueChanged</td><td>14680064</td><td>0x00E00000</td></tr>
<tr><td>GoodEdited</td><td>14417920</td><td>0x00DC0000</td></tr>
<tr><td>GoodEdited_DependentValueChanged</td><td>18219008</td><td>0x01160000</td></tr>
<tr><td>GoodEdited_DominantValueChanged</td><td>18284544</td><td>0x01170000</td></tr>
<tr><td>GoodEdited_DominantValueChanged_DependentValueChanged</td><td>18350080</td><td>0x01180000</td></tr>
<tr><td>GoodEntryInserted</td><td>10616832</td><td>0x00A20000</td></tr>
<tr><td>GoodEntryReplaced</td><td>10682368</td><td>0x00A30000</td></tr>
<tr><td>GoodFaultStateActive</td><td>67567616</td><td>0x04070000</td></tr>
<tr><td>GoodInitiateFaultState</td><td>67633152</td><td>0x04080000</td></tr>
<tr><td>GoodLocalOverride</td><td>9830400</td><td>0x00960000</td></tr>
<tr><td>GoodMoreData</td><td>10878976</td><td>0x00A60000</td></tr>
<tr><td>GoodNoData</td><td>10813440</td><td>0x00A50000</td></tr>
<tr><td>GoodNonCriticalTimeout</td><td>11141120</td><td>0x00AA0000</td></tr>
<tr><td>GoodOverload</td><td>3080192</td><td>0x002F0000</td></tr>
<tr><td>GoodPostActionFailed</td><td>14483456</td><td>0x00DD0000</td></tr>
<tr><td>GoodResultsMayBeIncomplete</td><td>12189696</td><td>0x00BA0000</td></tr>
<tr><td>GoodRetransmissionQueueNotSupported</td><td>14614528</td><td>0x00DF0000</td></tr>
<tr><td>GoodShutdownEvent</td><td>11010048</td><td>0x00A80000</td></tr>
<tr><td>GoodSubscriptionTransferred</td><td>2949120</td><td>0x002D0000</td></tr>
<tr><th>Uncertain</th><th></th><th>0x4...0000</th></tr>
<tr><td>Uncertain</td><td>1073741824</td><td>0x40000000</td></tr>
<tr><td>UncertainConfigurationError</td><td>1108279296</td><td>0x420F0000</td></tr>
<tr><td>UncertainDataSubNormal</td><td>1084489728</td><td>0x40A40000</td></tr>
<tr><td>UncertainDependentValueChanged</td><td>1088552960</td><td>0x40E20000</td></tr>
<tr><td>UncertainDominantValueChanged</td><td>1088290816</td><td>0x40DE0000</td></tr>
<tr><td>UncertainEngineeringUnitsExceeded</td><td>1083441152</td><td>0x40940000</td></tr>
<tr><td>UncertainInitialValue</td><td>1083310080</td><td>0x40920000</td></tr>
<tr><td>UncertainLastUsableValue</td><td>1083179008</td><td>0x40900000</td></tr>
<tr><td>UncertainNoCommunicationLastUsableValue</td><td>1083113472</td><td>0x408F0000</td></tr>
<tr><td>UncertainNotAllNodesAvailable</td><td>1086324736</td><td>0x40C00000</td></tr>
<tr><td>UncertainReferenceNotDeleted</td><td>1086062592</td><td>0x40BC0000</td></tr>
<tr><td>UncertainReferenceOutOfServer</td><td>1080819712</td><td>0x406C0000</td></tr>
<tr><td>UncertainSensorCalibration</td><td>1107951616</td><td>0x420A0000</td></tr>
<tr><td>UncertainSensorNotAccurate</td><td>1083375616</td><td>0x40930000</td></tr>
<tr><td>UncertainSimulatedValue</td><td>1107886080</td><td>0x42090000</td></tr>
<tr><td>UncertainSubNormal</td><td>1083506688</td><td>0x40950000</td></tr>
<tr><td>UncertainSubstituteValue</td><td>1083244544</td><td>0x40910000</td></tr>
<tr><td>UncertainTransducerInManual</td><td>1107820544</td><td>0x42080000</td></tr>
<tr><th>Bad</th><th></th><th>0x8...0000</th></tr>
<tr><td>Bad</td><td>2147483648</td><td>0x80000000</td></tr>
<tr><td>BadAggregateConfigurationRejected</td><td>2161770496</td><td>0x80DA0000</td></tr>
<tr><td>BadAggregateInvalidInputs</td><td>2161508352</td><td>0x80D60000</td></tr>
<tr><td>BadAggregateListMismatch</td><td>2161377280</td><td>0x80D40000</td></tr>
<tr><td>BadAggregateNotSupported</td><td>2161442816</td><td>0x80D50000</td></tr>
<tr><td>BadAlreadyExists</td><td>2165637120</td><td>0x81150000</td></tr>
<tr><td>BadApplicationSignatureInvalid</td><td>2153250816</td><td>0x80580000</td></tr>
<tr><td>BadArgumentsMissing</td><td>2155216896</td><td>0x80760000</td></tr>
<tr><td>BadAttributeIdInvalid</td><td>2150957056</td><td>0x80350000</td></tr>
<tr><td>BadBoundNotFound</td><td>2161573888</td><td>0x80D70000</td></tr>
<tr><td>BadBoundNotSupported</td><td>2161639424</td><td>0x80D80000</td></tr>
<tr><td>BadBrowseDirectionInvalid</td><td>2152529920</td><td>0x804D0000</td></tr>
<tr><td>BadBrowseNameDuplicated</td><td>2153840640</td><td>0x80610000</td></tr>
<tr><td>BadBrowseNameInvalid</td><td>2153775104</td><td>0x80600000</td></tr>
<tr><td>BadCertificateChainIncomplete</td><td>2165112832</td><td>0x810D0000</td></tr>
<tr><td>BadCertificateHostNameInvalid</td><td>2148925440</td><td>0x80160000</td></tr>
<tr><td>BadCertificateInvalid</td><td>2148663296</td><td>0x80120000</td></tr>
<tr><td>BadCertificateIssuerRevocationUnknown</td><td>2149318656</td><td>0x801C0000</td></tr>
<tr><td>BadCertificateIssuerRevoked</td><td>2149449728</td><td>0x801E0000</td></tr>
<tr><td>BadCertificateIssuerTimeInvalid</td><td>2148859904</td><td>0x80150000</td></tr>
<tr><td>BadCertificateIssuerUseNotAllowed</td><td>2149122048</td><td>0x80190000</td></tr>
<tr><td>BadCertificatePolicyCheckFailed</td><td>2165571584</td><td>0x81140000</td></tr>
<tr><td>BadCertificateRevocationUnknown</td><td>2149253120</td><td>0x801B0000</td></tr>
<tr><td>BadCertificateRevoked</td><td>2149384192</td><td>0x801D0000</td></tr>
<tr><td>BadCertificateTimeInvalid</td><td>2148794368</td><td>0x80140000</td></tr>
<tr><td>BadCertificateUntrusted</td><td>2149187584</td><td>0x801A0000</td></tr>
<tr><td>BadCertificateUriInvalid</td><td>2148990976</td><td>0x80170000</td></tr>
<tr><td>BadCertificateUseNotAllowed</td><td>2149056512</td><td>0x80180000</td></tr>
<tr><td>BadCommunicationError</td><td>2147811328</td><td>0x80050000</td></tr>
<tr><td>BadConditionAlreadyDisabled</td><td>2157445120</td><td>0x80980000</td></tr>
<tr><td>BadConditionAlreadyEnabled</td><td>2160852992</td><td>0x80CC0000</td></tr>
<tr><td>BadConditionAlreadyShelved</td><td>2161180672</td><td>0x80D10000</td></tr>
<tr><td>BadConditionBranchAlreadyAcked</td><td>2161049600</td><td>0x80CF0000</td></tr>
<tr><td>BadConditionBranchAlreadyConfirmed</td><td>2161115136</td><td>0x80D00000</td></tr>
<tr><td>BadConditionDisabled</td><td>2157510656</td><td>0x80990000</td></tr>
<tr><td>BadConditionNotShelved</td><td>2161246208</td><td>0x80D20000</td></tr>
<tr><td>BadConfigurationError</td><td>2156462080</td><td>0x80890000</td></tr>
<tr><td>BadConnectionClosed</td><td>2158886912</td><td>0x80AE0000</td></tr>
<tr><td>BadConnectionRejected</td><td>2158755840</td><td>0x80AC0000</td></tr>
<tr><td>BadContentFilterInvalid</td><td>2152202240</td><td>0x80480000</td></tr>
<tr><td>BadContinuationPointInvalid</td><td>2152333312</td><td>0x804A0000</td></tr>
<tr><td>BadDataEncodingInvalid</td><td>2151153664</td><td>0x80380000</td></tr>
<tr><td>BadDataEncodingUnsupported</td><td>2151219200</td><td>0x80390000</td></tr>
<tr><td>BadDataLost</td><td>2157772800</td><td>0x809D0000</td></tr>
<tr><td>BadDataTypeIdUnknown</td><td>2148597760</td><td>0x80110000</td></tr>
<tr><td>BadDataUnavailable</td><td>2157838336</td><td>0x809E0000</td></tr>
<tr><td>BadDeadbandFilterInvalid</td><td>2156789760</td><td>0x808E0000</td></tr>
<tr><td>BadDecodingError</td><td>2147942400</td><td>0x80070000</td></tr>
<tr><td>BadDependentValueChanged</td><td>2162360320</td><td>0x80E30000</td></tr>
<tr><td>BadDeviceFailure</td><td>2156593152</td><td>0x808B0000</td></tr>
<tr><td>BadDialogNotActive</td><td>2160918528</td><td>0x80CD0000</td></tr>
<tr><td>BadDialogResponseInvalid</td><td>2160984064</td><td>0x80CE0000</td></tr>
<tr><td>BadDisconnect</td><td>2158821376</td><td>0x80AD0000</td></tr>
<tr><td>BadDiscoveryUrlMissing</td><td>2152792064</td><td>0x80510000</td></tr>
<tr><td>BadDominantValueChanged</td><td>2162229248</td><td>0x80E10000</td></tr>
<tr><td>BadDuplicateReferenceNotAllowed</td><td>2154168320</td><td>0x80660000</td></tr>
<tr><td>BadEdited_OutOfRange</td><td>2165899264</td><td>0x81190000</td></tr>
<tr><td>BadEdited_OutOfRange_DominantValueChanged</td><td>2166095872</td><td>0x811C0000</td></tr>
<tr><td>BadEdited_OutOfRange_DominantValueChanged_DependentValueChanged</td><td>2166226944</td><td>0x811E0000</td></tr>
<tr><td>BadEncodingError</td><td>2147876864</td><td>0x80060000</td></tr>
<tr><td>BadEncodingLimitsExceeded</td><td>2148007936</td><td>0x80080000</td></tr>
<tr><td>BadEndOfStream</td><td>2159017984</td><td>0x80B00000</td></tr>
<tr><td>BadEntryExists</td><td>2157903872</td><td>0x809F0000</td></tr>
<tr><td>BadEventFilterInvalid</td><td>2152136704</td><td>0x80470000</td></tr>
<tr><td>BadEventIdUnknown</td><td>2157576192</td><td>0x809A0000</td></tr>
<tr><td>BadEventNotAcknowledgeable</td><td>2159738880</td><td>0x80BB0000</td></tr>
<tr><td>BadExpectedStreamToBlock</td><td>2159280128</td><td>0x80B40000</td></tr>
<tr><td>BadFilterElementInvalid</td><td>2160328704</td><td>0x80C40000</td></tr>
<tr><td>BadFilterLiteralInvalid</td><td>2160394240</td><td>0x80C50000</td></tr>
<tr><td>BadFilterNotAllowed</td><td>2152005632</td><td>0x80450000</td></tr>
<tr><td>BadFilterOperandCountMismatch</td><td>2160263168</td><td>0x80C30000</td></tr>
<tr><td>BadFilterOperandInvalid</td><td>2152267776</td><td>0x80490000</td></tr>
<tr><td>BadFilterOperatorInvalid</td><td>2160132096</td><td>0x80C10000</td></tr>
<tr><td>BadFilterOperatorUnsupported</td><td>2160197632</td><td>0x80C20000</td></tr>
<tr><td>BadHistoryOperationInvalid</td><td>2154889216</td><td>0x80710000</td></tr>
<tr><td>BadHistoryOperationUnsupported</td><td>2154954752</td><td>0x80720000</td></tr>
<tr><td>BadIdentityChangeNotSupported</td><td>2160459776</td><td>0x80C60000</td></tr>
<tr><td>BadIdentityTokenInvalid</td><td>2149580800</td><td>0x80200000</td></tr>
<tr><td>BadIdentityTokenRejected</td><td>2149646336</td><td>0x80210000</td></tr>
<tr><td>BadIndexRangeInvalid</td><td>2151022592</td><td>0x80360000</td></tr>
<tr><td>BadIndexRangeNoData</td><td>2151088128</td><td>0x80370000</td></tr>
<tr><td>BadInitialValue_OutOfRange</td><td>2165964800</td><td>0x811A0000</td></tr>
<tr><td>BadInsufficientClientProfile</td><td>2155610112</td><td>0x807C0000</td></tr>
<tr><td>BadInternalError</td><td>2147614720</td><td>0x80020000</td></tr>
<tr><td>BadInvalidArgument</td><td>2158690304</td><td>0x80AB0000</td></tr>
<tr><td>BadInvalidSelfReference</td><td>2154233856</td><td>0x80670000</td></tr>
<tr><td>BadInvalidState</td><td>2158952448</td><td>0x80AF0000</td></tr>
<tr><td>BadInvalidTimestamp</td><td>2149777408</td><td>0x80230000</td></tr>
<tr><td>BadInvalidTimestampArgument</td><td>2159869952</td><td>0x80BD0000</td></tr>
<tr><td>BadLicenseExpired</td><td>2165178368</td><td>0x810E0000</td></tr>
<tr><td>BadLicenseLimitsExceeded</td><td>2165243904</td><td>0x810F0000</td></tr>
<tr><td>BadLicenseNotAvailable</td><td>2165309440</td><td>0x81100000</td></tr>
<tr><td>BadMaxAgeInvalid</td><td>2154823680</td><td>0x80700000</td></tr>
<tr><td>BadMaxConnectionsReached</td><td>2159476736</td><td>0x80B70000</td></tr>
<tr><td>BadMessageNotAvailable</td><td>2155544576</td><td>0x807B0000</td></tr>
<tr><td>BadMethodInvalid</td><td>2155151360</td><td>0x80750000</td></tr>
<tr><td>BadMonitoredItemFilterInvalid</td><td>2151874560</td><td>0x80430000</td></tr>
<tr><td>BadMonitoredItemFilterUnsupported</td><td>2151940096</td><td>0x80440000</td></tr>
<tr><td>BadMonitoredItemIdInvalid</td><td>2151809024</td><td>0x80420000</td></tr>
<tr><td>BadMonitoringModeInvalid</td><td>2151743488</td><td>0x80410000</td></tr>
<tr><td>BadNoCommunication</td><td>2150694912</td><td>0x80310000</td></tr>
<tr><td>BadNoContinuationPoints</td><td>2152398848</td><td>0x804B0000</td></tr>
<tr><td>BadNoData</td><td>2157641728</td><td>0x809B0000</td></tr>
<tr><td>BadNoDataAvailable</td><td>2159083520</td><td>0x80B10000</td></tr>
<tr><td>BadNoDeleteRights</td><td>2154364928</td><td>0x80690000</td></tr>
<tr><td>BadNoEntryExists</td><td>2157969408</td><td>0x80A00000</td></tr>
<tr><td>BadNoMatch</td><td>2154758144</td><td>0x806F0000</td></tr>
<tr><td>BadNoSubscription</td><td>2155413504</td><td>0x80790000</td></tr>
<tr><td>BadNoValidCertificates</td><td>2153316352</td><td>0x80590000</td></tr>
<tr><td>BadNodeAttributesInvalid</td><td>2153906176</td><td>0x80620000</td></tr>
<tr><td>BadNodeClassInvalid</td><td>2153709568</td><td>0x805F0000</td></tr>
<tr><td>BadNodeIdExists</td><td>2153644032</td><td>0x805E0000</td></tr>
<tr><td>BadNodeIdInvalid</td><td>2150825984</td><td>0x80330000</td></tr>
<tr><td>BadNodeIdRejected</td><td>2153578496</td><td>0x805D0000</td></tr>
<tr><td>BadNodeIdUnknown</td><td>2150891520</td><td>0x80340000</td></tr>
<tr><td>BadNodeNotInView</td><td>2152595456</td><td>0x804E0000</td></tr>
<tr><td>BadNonceInvalid</td><td>2149842944</td><td>0x80240000</td></tr>
<tr><td>BadNotConnected</td><td>2156527616</td><td>0x808A0000</td></tr>
<tr><td>BadNotExecutable</td><td>2165374976</td><td>0x81110000</td></tr>
<tr><td>BadNotFound</td><td>2151546880</td><td>0x803E0000</td></tr>
<tr><td>BadNotImplemented</td><td>2151677952</td><td>0x80400000</td></tr>
<tr><td>BadNotReadable</td><td>2151284736</td><td>0x803A0000</td></tr>
<tr><td>BadNotSupported</td><td>2151481344</td><td>0x803D0000</td></tr>
<tr><td>BadNotTypeDefinition</td><td>2160590848</td><td>0x80C80000</td></tr>
<tr><td>BadNotWritable</td><td>2151350272</td><td>0x803B0000</td></tr>
<tr><td>BadNothingToDo</td><td>2148466688</td><td>0x800F0000</td></tr>
<tr><td>BadNumericOverflow</td><td>2165440512</td><td>0x81120000</td></tr>
<tr><td>BadObjectDeleted</td><td>2151612416</td><td>0x803F0000</td></tr>
<tr><td>BadOperationAbandoned</td><td>2159214592</td><td>0x80B30000</td></tr>
<tr><td>BadOutOfMemory</td><td>2147680256</td><td>0x80030000</td></tr>
<tr><td>BadOutOfRange</td><td>2151415808</td><td>0x803C0000</td></tr>
<tr><td>BadOutOfRange_DominantValueChanged</td><td>2166030336</td><td>0x811B0000</td></tr>
<tr><td>BadOutOfRange_DominantValueChanged_DependentValueChanged</td><td>2166161408</td><td>0x811D0000</td></tr>
<tr><td>BadOutOfService</td><td>2156724224</td><td>0x808D0000</td></tr>
<tr><td>BadParentNodeIdInvalid</td><td>2153447424</td><td>0x805B0000</td></tr>
<tr><td>BadProtocolVersionUnsupported</td><td>2159935488</td><td>0x80BE0000</td></tr>
<tr><td>BadQueryTooComplex</td><td>2154692608</td><td>0x806E0000</td></tr>
<tr><td>BadReferenceLocalOnly</td><td>2154299392</td><td>0x80680000</td></tr>
<tr><td>BadReferenceNotAllowed</td><td>2153512960</td><td>0x805C0000</td></tr>
<tr><td>BadReferenceTypeIdInvalid</td><td>2152464384</td><td>0x804C0000</td></tr>
<tr><td>BadRefreshInProgress</td><td>2157379584</td><td>0x80970000</td></tr>
<tr><td>BadRequestCancelledByClient</td><td>2150367232</td><td>0x802C0000</td></tr>
<tr><td>BadRequestCancelledByRequest</td><td>2153381888</td><td>0x805A0000</td></tr>
<tr><td>BadRequestHeaderInvalid</td><td>2150236160</td><td>0x802A0000</td></tr>
<tr><td>BadRequestInterrupted</td><td>2156134400</td><td>0x80840000</td></tr>
<tr><td>BadRequestNotAllowed</td><td>2162425856</td><td>0x80E40000</td></tr>
<tr><td>BadRequestNotComplete</td><td>2165506048</td><td>0x81130000</td></tr>
<tr><td>BadRequestTimeout</td><td>2156199936</td><td>0x80850000</td></tr>
<tr><td>BadRequestTooLarge</td><td>2159542272</td><td>0x80B80000</td></tr>
<tr><td>BadRequestTypeInvalid</td><td>2152923136</td><td>0x80530000</td></tr>
<tr><td>BadResourceUnavailable</td><td>2147745792</td><td>0x80040000</td></tr>
<tr><td>BadResponseTooLarge</td><td>2159607808</td><td>0x80B90000</td></tr>
<tr><td>BadSecureChannelClosed</td><td>2156265472</td><td>0x80860000</td></tr>
<tr><td>BadSecureChannelIdInvalid</td><td>2149711872</td><td>0x80220000</td></tr>
<tr><td>BadSecureChannelTokenUnknown</td><td>2156331008</td><td>0x80870000</td></tr>
<tr><td>BadSecurityChecksFailed</td><td>2148728832</td><td>0x80130000</td></tr>
<tr><td>BadSecurityModeInsufficient</td><td>2162556928</td><td>0x80E60000</td></tr>
<tr><td>BadSecurityModeRejected</td><td>2152988672</td><td>0x80540000</td></tr>
<tr><td>BadSecurityPolicyRejected</td><td>2153054208</td><td>0x80550000</td></tr>
<tr><td>BadSempahoreFileMissing</td><td>2152857600</td><td>0x80520000</td></tr>
<tr><td>BadSensorFailure</td><td>2156658688</td><td>0x808C0000</td></tr>
<tr><td>BadSequenceNumberInvalid</td><td>2156396544</td><td>0x80880000</td></tr>
<tr><td>BadSequenceNumberUnknown</td><td>2155479040</td><td>0x807A0000</td></tr>
<tr><td>BadServerHalted</td><td>2148401152</td><td>0x800E0000</td></tr>
<tr><td>BadServerIndexInvalid</td><td>2154430464</td><td>0x806A0000</td></tr>
<tr><td>BadServerNameMissing</td><td>2152726528</td><td>0x80500000</td></tr>
<tr><td>BadServerNotConnected</td><td>2148335616</td><td>0x800D0000</td></tr>
<tr><td>BadServerUriInvalid</td><td>2152660992</td><td>0x804F0000</td></tr>
<tr><td>BadServiceUnsupported</td><td>2148204544</td><td>0x800B0000</td></tr>
<tr><td>BadSessionClosed</td><td>2149974016</td><td>0x80260000</td></tr>
<tr><td>BadSessionIdInvalid</td><td>2149908480</td><td>0x80250000</td></tr>
<tr><td>BadSessionNotActivated</td><td>2150039552</td><td>0x80270000</td></tr>
<tr><td>BadShelvingTimeOutOfRange</td><td>2161311744</td><td>0x80D30000</td></tr>
<tr><td>BadShutdown</td><td>2148270080</td><td>0x800C0000</td></tr>
<tr><td>BadSourceNodeIdInvalid</td><td>2154037248</td><td>0x80640000</td></tr>
<tr><td>BadStateNotActive</td><td>2160001024</td><td>0x80BF0000</td></tr>
<tr><td>BadStructureMissing</td><td>2152071168</td><td>0x80460000</td></tr>
<tr><td>BadSubscriptionIdInvalid</td><td>2150105088</td><td>0x80280000</td></tr>
<tr><td>BadSyntaxError</td><td>2159411200</td><td>0x80B60000</td></tr>
<tr><td>BadTargetNodeIdInvalid</td><td>2154102784</td><td>0x80650000</td></tr>
<tr><td>BadTcpEndpointUrlInvalid</td><td>2156068864</td><td>0x80830000</td></tr>
<tr><td>BadTcpInternalError</td><td>2156003328</td><td>0x80820000</td></tr>
<tr><td>BadTcpMessageTooLarge</td><td>2155872256</td><td>0x80800000</td></tr>
<tr><td>BadTcpMessageTypeInvalid</td><td>2155741184</td><td>0x807E0000</td></tr>
<tr><td>BadTcpNotEnoughResources</td><td>2155937792</td><td>0x80810000</td></tr>
<tr><td>BadTcpSecureChannelUnknown</td><td>2155806720</td><td>0x807F0000</td></tr>
<tr><td>BadTcpServerTooBusy</td><td>2155675648</td><td>0x807D0000</td></tr>
<tr><td>BadTicketInvalid</td><td>2166358016</td><td>0x81200000</td></tr>
<tr><td>BadTicketRequired</td><td>2166292480</td><td>0x811F0000</td></tr>
<tr><td>BadTimeout</td><td>2148139008</td><td>0x800A0000</td></tr>
<tr><td>BadTimestampNotSupported</td><td>2158034944</td><td>0x80A10000</td></tr>
<tr><td>BadTimestampsToReturnInvalid</td><td>2150301696</td><td>0x802B0000</td></tr>
<tr><td>BadTooManyArguments</td><td>2162491392</td><td>0x80E50000</td></tr>
<tr><td>BadTooManyMatches</td><td>2154627072</td><td>0x806D0000</td></tr>
<tr><td>BadTooManyMonitoredItems</td><td>2161836032</td><td>0x80DB0000</td></tr>
<tr><td>BadTooManyOperations</td><td>2148532224</td><td>0x80100000</td></tr>
<tr><td>BadTooManyPublishRequests</td><td>2155347968</td><td>0x80780000</td></tr>
<tr><td>BadTooManySessions</td><td>2153119744</td><td>0x80560000</td></tr>
<tr><td>BadTooManySubscriptions</td><td>2155282432</td><td>0x80770000</td></tr>
<tr><td>BadTypeDefinitionInvalid</td><td>2153971712</td><td>0x80630000</td></tr>
<tr><td>BadTypeMismatch</td><td>2155085824</td><td>0x80740000</td></tr>
<tr><td>BadUnexpectedError</td><td>2147549184</td><td>0x80010000</td></tr>
<tr><td>BadUnknownResponse</td><td>2148073472</td><td>0x80090000</td></tr>
<tr><td>BadUserAccessDenied</td><td>2149515264</td><td>0x801F0000</td></tr>
<tr><td>BadUserSignatureInvalid</td><td>2153185280</td><td>0x80570000</td></tr>
<tr><td>BadViewIdUnknown</td><td>2154496000</td><td>0x806B0000</td></tr>
<tr><td>BadViewParameterMismatch</td><td>2160721920</td><td>0x80CA0000</td></tr>
<tr><td>BadViewTimestampInvalid</td><td>2160656384</td><td>0x80C90000</td></tr>
<tr><td>BadViewVersionInvalid</td><td>2160787456</td><td>0x80CB0000</td></tr>
<tr><td>BadWaitingForInitialData</td><td>2150760448</td><td>0x80320000</td></tr>
<tr><td>BadWaitingForResponse</td><td>2159149056</td><td>0x80B20000</td></tr>
<tr><td>BadWouldBlock</td><td>2159345664</td><td>0x80B50000</td></tr>
<tr><td>BadWriteNotSupported</td><td>2155020288</td><td>0x80730000</td></tr>
</table>
</details>

## Modifier flags
<details>
<summary>Click to see full list of modifiers</summary>
<table>
<tr><th>Modifier symbolic representation</th><th>Code (decimal)</th><th>Code (hexadecimal)</th></tr>
<tr><td>, StructureChanged</td><td>32768</td><td>0x8000</td></tr>
<tr><td>, SemanticsChanged</td><td>16384</td><td>0x4000</td></tr>
<tr><td colspan="3">Add the following number if and only if any of the below <i>info flags</i> are added</td></tr>
<tr><td></td><td>1024</td><td>0x0400</td></tr>
<tr><td colspan="3">Only one of the following 3 limit flags may be set.</td></tr>
<tr><td>, Constant</td><td>768</td><td>0x0300</td></tr>
<tr><td>, High</td><td>512</td><td>0x0200</td></tr>
<tr><td>, Low</td><td>256</td><td>0x0100</td></tr>
<tr><td colspan="3">Other info flags:</td></tr>
<tr><td>, Overflow</td><td>128</td><td>0x0080</td></tr>
<tr><td>, MultipleValues</td><td>16</td><td>0x0010</td></tr>
<tr><td>, ExtraData</td><td>8</td><td>0x0008</td></tr>
<tr><td>, Partial</td><td>4</td><td>0x0004</td></tr>
<tr><td colspan="3">Only one of interpolated and calculated can be set.</td></tr>
<tr><td>, Interpolated</td><td>2</td><td>0x0002</td></tr>
<tr><td>, Calculated</td><td>1</td><td>0x0001</td></tr>
</table>
</details>

## API Examples

### Ingestion

The following request ingest has five data points: three good, one uncertain, and one bad:

```json
POST /api/v1/projects/{project}/timeseries/data
Content-Type: application/json

{
    "items": [
        {
            "externalId": "outside-temperature",
            "datapoints": [
                {
                    "timestamp": 1620000000000,
                    "value": 1
                },
                {
                    "timestamp": 1620000000001,
                    "value": 2,
                    "status": {
                        "code": 0
                    }
                },
                {
                    "timestamp": 1620000000002,
                    "value": 3,
                    "status": {
                        "symbol": "GoodClamped"
                    }
                },
                {
                    "timestamp": 1620000000003,
                    "value": 4,
                    "status": {
                        "symbol": "Uncertain"
                    }
                },
                {
                    "timestamp": 1620000000004,
                    "value": 5,
                    "status": {
                        "code": 2153809152
                    }
                }
            ]
        }
    ]
}
```

### Retrieving raw data points

To be able to see the status codes on the data points we retrieve, we must set the `includeStatus` parameter to true.

```json
POST /api/v1/projects/{project}/timeseries/data/list
Content-Type: application/json

{
    "items": [
        {
            "externalId": "outside-temperature",
            "includeStatus": true,
        }
    ]
}
```

With the example data from above, the response looks like this:

```json
{
    "items": [
        {
            // ...
            "datapoints": [
                {
                    "timestamp": 1620000000000,
                    "value": 1.0
                },
                {
                    "timestamp": 1620000000001,
                    "value": 2.0
                },
                {
                    "timestamp": 1620000000002,
                    "value": 3.0,
                    "status": {
                        "code": 3145728,
                        "symbol": "GoodClamped"
                    }
                }
            ]
        }
    ]
}
```

Notice that we don't get the status code for the first two data points because they are exactly `Good` with `code = 0`.
They are omitted as a performance optimization since most status codes are 0.
For the last data point, we get both the code and the symbolic string representation.

#### List all data points

To list all data points, we can set the `ignoreBadDataPoints` parameter to `false`.
Because the API treats _uncertain_ data points as _bad_ by default, this parameter includes both _uncertain_ and _bad_ data points.

```json
POST /api/v1/projects/{project}/timeseries/data/list
Content-Type: application/json

{
    "items": [
        {
            "externalId": "outside-temperature",
            "includeStatus": true,
            "ignoreBadDataPoints": false,
        }
    ]
}
```

Using the example data from above, the response looks like this:

```json
{
    "items": [
        {
            // ...
            "datapoints": [
                {
                    "timestamp": 1620000000000,
                    "value": 1.0
                },
                {
                    "timestamp": 1620000000001,
                    "value": 2.0
                },
                {
                    "timestamp": 1620000000002,
                    "value": 3.0,
                    "status": {
                        "code": 3145728,
                        "symbol": "GoodClamped"
                    }
                },
                {
                    "timestamp": 1620000000003,
                    "value": 4.0,
                    "status": {
                        "code": 1073741824,
                        "symbol": "Uncertain"
                    }
                },
                {
                    "timestamp": 1620000000004,
                    "value": 5.0,
                    "status": {
                        "code": 2153809152,
                        "symbol": "BadBrowseNameInvalid, StructureChanged, Low"
                    }
                }
            ]
        }
    ]
}
```

### Retrieving aggregates

The example below requests seven aggregates.
The first aggregate is the number of data points included that are used in the calculations.
The next three are simple counts of the number of data points with each status code present.
The last three are the average, min, and max for only the good data points.

```json
POST /api/v1/projects/{project}/timeseries/data/list
Content-Type: application/json

{
    "items": [
        {
            "externalId": "outside-temperature",
            "granularity": "1s",
            "aggregates": [
                "count",
                "countGood", "countUncertain", "countBad",
                "average", "min", "max"
            ]
        }
    ]
}
```

Using the example data from above, the response looks like this:

```json
{
    "items": [
        {
            // ...
            "datapoints": [
                {
                    "timestamp": 1620000000000,
                    "count": 3,
                    "countGood": 3,
                    "countUncertain": 1,
                    "countBad": 1,
                    "average": 2.0,
                    "min": 1.0,
                    "max": 3.0
                }
            ]
        }
    ]
}
```

<Note>
The example doesn't include parameters to take uncertain and bad datapoints into account.
Therefore, the count, average, min, and max calculations are performed ignoring uncertain and bad data points.
</Note>

#### Treating uncertain data points as good

To also include the uncertain data points as if they were good, you can set the `treateUncertainAsBad` parameter to `false`.
This parameter is also available for raw data points.

```json
POST /api/v1/projects/{project}/timeseries/data/list
Content-Type: application/json

{
    "items": [
        {
            "externalId": "outside-temperature",
            "granularity": "1s",
            "treatUncertainAsBad": false,
            "aggregates": [
                "count",
                "countGood", "countUncertain", "countBad",
                "average", "min", "max"
            ]
        }
    ]
}
```

Using the example data from above, the response looks like this:

```json
{
    "items": [
        {
            // ...
            "datapoints": [
                {
                    "timestamp": 1620000000000,
                    "count": 4,
                    "countGood": 3,
                    "countUncertain": 1,
                    "countBad": 1,
                    "average": 2.5,
                    "min": 1.0,
                    "max": 4.0
                }
            ]
        }
    ]
}
```

Treating the _uncertain_ data point as _good_ increases the total count to four.
Notice that also the `count`, `average`, `min`, and `max` values have changed because of the additional good data point in the calculation.

## Aggregate behavior

There are three boolean parameters that deterimine how aggregates are calculated:
`isStep`, `treatUncertainAsBad` and `ignoreBadDataPoints`.
Together, they result in 8 different combinations that gives different aggregate behavior.

- If `treatUncertainAsBad` is true, uncertain data points are treated as being bad (the default).
- If `ignoreBadDataPoints` is true, data points that are treated as bad will be ignored (the default).
  If this parameter is set to false, interpolation will stop at the beginning of each bad period.
- if the time series is configured with `isStep` set to true, then there will be no interpolation between data points.
   The last value is always _carried forward_ until the next data point.

The image below illustrates how we interpolate between data points with different status codes in each of the 8 combinations.

<img src="/images/cdf/dev/concepts/aggregation/drawing_with_status_codes.png" alt="" />
