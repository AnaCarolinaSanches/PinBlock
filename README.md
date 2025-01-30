LibGit2Sharp.NotFoundException: No valid git object identified by 'refs/remotes/origin/feature/APP-439-tela-cancelamento-iof' exists in the repository.
   at LibGit2Sharp.Core.Ensure.GitObjectIsNotNull(GitObject gitObject, String identifier) in /_/LibGit2Sharp/Core/Ensure.cs:line 265
   at LibGit2Sharp.RepositoryExtensions.SingleCommittish(Repository repo, Object identifier) in /_/LibGit2Sharp/RepositoryExtensions.cs:line 288
   at LibGit2Sharp.RepositoryExtensions.Committishes(Repository repo, Object identifier, Boolean throwIfNotFound)+MoveNext() in /_/LibGit2Sharp/RepositoryExtensions.cs:line 310
   at LibGit2Sharp.RepositoryExtensions.Committishes(Repository repo, Object identifier, Boolean throwIfNotFound)+MoveNext() in /_/LibGit2Sharp/RepositoryExtensions.cs:line 322
   at System.Linq.Enumerable.TakeWhileIterator[TSource](IEnumerable`1 source, Func`2 predicate)+MoveNext()
   at LibGit2Sharp.CommitLog.CommitEnumerator.InternalHidePush(IList`1 identifier, HidePushSignature hidePush) in /_/LibGit2Sharp/CommitLog.cs:line 176
   at LibGit2Sharp.CommitLog.CommitEnumerator.Push(IList`1 identifier) in /_/LibGit2Sharp/CommitLog.cs:line 184
   at LibGit2Sharp.CommitLog.CommitEnumerator..ctor(Repository repo, CommitFilter filter) in /_/LibGit2Sharp/CommitLog.cs:line 121
   at LibGit2Sharp.CommitLog.GetEnumerator() in /_/LibGit2Sharp/CommitLog.cs:line 54
   at System.Linq.Enumerable.Contains[TSource](IEnumerable`1 source, TSource value, IEqualityComparer`1 comparer)
   at Mendix.Modeler.VersionControl.Actions.GitFetchAction.<>c__DisplayClass8_0.<GetNonPushedCommitNotes>b__0(Commit c) in Mendix.Modeler.VersionControl\Actions\Git\GitFetchAction.cs:line 49
   at System.Linq.Enumerable.TryGetFirst[TSource](IEnumerable`1 source, Func`2 predicate, Boolean& found)
   at Mendix.Modeler.VersionControl.Actions.GitFetchAction.GetNonPushedCommitNotes(Repository repo) in Mendix.Modeler.VersionControl\Actions\Git\GitFetchAction.cs:line 49
   at Mendix.Modeler.VersionControl.Actions.GitFetchAction.Execute(Repository repo) in Mendix.Modeler.VersionControl\Actions\Git\GitFetchAction.cs:line 86
   at Mendix.Modeler.VersionControl.Operations.Git.GitCheckVersionOperation.Execute(VersionCheckRequest request) in Mendix.Modeler.VersionControl\Operations\Git\GitCheckVersionOperation.cs:line 34
   at Mendix.Modeler.VersionControl.UpdateWorker.CheckVersion(ICheckConflictState state, IRemoteRepository remoteRepository, IRevLine branchLine, Revision revision, Boolean oneRevisionEarlier, String revisionDescription) in Mendix.Modeler.VersionControl\Update\UpdateWorker.cs:line 53
   at Mendix.Modeler.VersionControl.Updater.<>c__DisplayClass20_0.<AddCheckVersionStep>b__0(IProgressInfo _) in Mendix.Modeler.VersionControl\Update\Updater.cs:line 198
   at Mendix.Modeler.VersionControl.Updater.<>c__DisplayClass29_0.<AddProcessStep>b__0(IProgressInfo info) in Mendix.Modeler.VersionControl\Update\Updater.cs:line 308
   at Mendix.Modeler.UIFramework.Progress.ProcessRunner.RunStep(IStep step) in Mendix.Modeler.UIFramework\Progress\ProcessRunner.cs:line 160
   at Mendix.Modeler.UIFramework.Progress.ProcessRunner.OnDoWork(Object sender, DoWorkEventArgs e) in Mendix.Modeler.UIFramework\Progress\ProcessRunner.cs:line 81
   at System.ComponentModel.BackgroundWorker.WorkerThreadStart(Object argument)
--- End of stack trace from previous location ---
   at Mendix.Modeler.UIFramework.Progress.ProcessRunner.Run() in Mendix.Modeler.UIFramework\Progress\ProcessRunner.cs:line 57
   at Mendix.Modeler.VersionControl.Updater.LaunchUpdateProcess(RevUpdateState state) in Mendix.Modeler.VersionControl\Update\Updater.cs:line 151
   at Mendix.Modeler.VersionControl.Updater.Update(RevUpdateState state) in Mendix.Modeler.VersionControl\Update\Updater.cs:line 81
   at Mendix.Modeler.VersionControl.UpdaterUI.Update(IProject project, RevOperationSource source) in Mendix.Modeler.VersionControl.View\Update\UpdaterUI.cs:line 68
   at Mendix.Modeler.ProjectHandling.View.Commands.VersionControl.UpdateCommand.Call(IFeatureCommandContext context) in Mendix.Modeler.ProjectHandling.View\Commands\VersionControl\UpdateCommand.cs:line 25
   at Mendix.Modeler.Core.Commands.FeatureCommand.Execute(IFeatureCommandContext context) in Mendix.Modeler.Core\Commands\FeatureCommand.cs:line 88
   at Mendix.Modeler.MainWindow.Menus.VersionControlMenuItemsProvider.<>c__DisplayClass4_0.<MakeCommandHandler>b__0() in Mendix.Modeler.ProjectHandling.View\Menus\VersionControlMenuItemsProvider.cs:line 56
   at Eto.Forms.MenuItem.Callback.OnClick(MenuItem widget, EventArgs e)
   at Eto.Wpf.Forms.Menu.MenuItemHandler`3.OnClick()
   at Eto.Wpf.Forms.Menu.MenuItemHandler`3.<Initialize>b__1_0(Object sender, RoutedEventArgs e)
   at System.Windows.EventRoute.InvokeHandlersImpl(Object source, RoutedEventArgs args, Boolean reRaised)
   at System.Windows.UIElement.RaiseEventImpl(DependencyObject sender, Ro
